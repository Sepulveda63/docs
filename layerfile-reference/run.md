# RUN 

`RUN (BACKGROUND|REPEATABLE) [command ...]`

The `RUN` instruction runs the given script, and fails the entire Layerfile if the given command fails.

For example, you might use `RUN echo "the directory is $(pwd)"` to print your current directory.

### Examples

- `RUN echo hello` prints "hello" to the terminal
- `RUN BACKGROUND python3 -m http.server` run `python3 -m http.server` persistently in the background.
- `RUN REPEATABLE docker build -t hello` is a performance optimization, see [tuning performance](/docs/tuning-performance#run-repeatable)

<br />