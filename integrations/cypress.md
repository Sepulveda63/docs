![Cypress Logo](/docs/resources/cypress_logo.png)

# Cypress 

[Cypress](https://www.cypress.io/) is a fast, easy, and reliable end-to-end testing solution for anything that runs in a browser. 

## Example Layerfile

<pre>
    <code class="language-html CodeHighlight">
        #This is an example webapp.io configuration for React!
        FROM vm/ubuntu:18.04
        
        # To note: Layerfiles create entire VMs, *not* containers!
        
        RUN curl -fSsL https://deb.nodesource.com/setup_12.x | bash && \
            apt-get install nodejs python3 make gcc build-essential
        
        # Cypress OS dependencies
        # https://on.cypress.io/ci#Dependencies
        RUN apt-get install libgtk2.0-0 libgtk-3-0 libgbm-dev \
            libnotify-dev libgconf-2-4 libnss3 libxss1 \
            libasound2 libxtst6 xauth xvfb
        
        # node is a memory hog
        MEMORY 2G
        ENV NODE_OPTIONS=--max-old-space-size=8192
        
        RUN node --version
        RUN npm --version
        
        # Typically we would need to cache
        # the NPM modules and the Cypress binary folder
        # in the folders ~/.npm and ~/.cache/Cypress
        # but webapp.io literally makes and stores Docker containers
        # after each command, thus if there were no changes
        # the layers would not be rebuilt
        # BUT we still want to cache some folders to avoid
        # reinstalling Cypress for example when just some source files have changed
        # https://webapp.io/docs/layerfile-reference/cache#cache
        COPY package.json ./
        COPY package-lock.json ./
        CACHE ~/.npm ~/.cache/Cypress
        
        # copy the rest of the files
        COPY . .
        RUN npm ci
        
        # shows where Cypress was installed and what's available
        RUN npx cypress info
        
        # print webapp.io variables
        # https://webapp.io/docs/layerfile-reference/build-env
        # using https://github.com/bahmutov/print-env
        # https://github.com/cypress-io/cypress/issues/16101
        RUN npx @bahmutov/print-env GIT CI RETRY USER SPLIT
        
        RUN BACKGROUND npm start
        
        # Create a unique link to share the app in this runner.
        # Every time someone clicks the link, we'll wake up this staging server.
        EXPOSE WEBSITE http://localhost:8888
        
        SECRET ENV CYPRESS_RECORD_KEY
        
        # execute the tests against the same machine (?) on N machines
        SPLIT 3
        RUN npx cypress run --record --parallel --ci-build-id $JOB_ID-$RETRY_INDEX
    </code>
</pre>

<br />

### Setting up Cypress with webapp.io

[Documentation](https://docs.cypress.io/guides/continuous-integration/introduction#Setting-up-CI) on how to run Cypress inside a CI pipeline and an [example project](https://github.com/bahmutov/cypress-example-layerci) running Cypress on webapp.io are also available.
