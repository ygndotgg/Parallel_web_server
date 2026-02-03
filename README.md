# Parallel Web Server

A high-performance multithreaded HTTP web server written in Rust, built for handling multiple concurrent connections efficiently using a thread pool architecture.

## Features

- **Multithreaded Architecture**: Uses a thread pool with worker threads to handle multiple concurrent client connections
- **Graceful Shutdown**: Supports Ctrl-C interruption with proper cleanup
- **Request Statistics**: Tracks and reports statistics about handled requests
- **Response Caching**: Implements caching mechanism for improved performance
- **Static File Serving**: Serves HTML files with appropriate responses
- **Error Handling**: Returns proper HTTP 404 responses for not found resources

## Project Structure

```
src/
├── main.rs           # Entry point - sets up thread pool, listener, and reporters
├── lib.rs            # Library exports
├── thread_pool.rs    # Thread pool implementation for managing worker threads
├── tcp.rs            # TCP listener with cancellation support
├── handler.rs        # HTTP request handler
├── cache.rs          # Caching mechanism for responses
└── statistic.rs      # Statistics collection and reporting
```

## Prerequisites

- Rust 1.70 or later

## Building

```bash
cargo build
```

## Running

```bash
cargo run
```

The server will start listening on `localhost:7878` and print:
```
Run `curl http://localhost:7878/KEY` to query the server with KEY
```

## Usage

### Testing with curl

```bash
# Request a simple response
curl http://localhost:7878/hello

# Request with sleep delay
curl http://localhost:7878/sleep

# Request a non-existent resource (404)
curl http://localhost:7878/notfound
```

### Browser Access

Open your browser and navigate to:
- `http://localhost:7878/hello` - Hello page
- `http://localhost:7878/sleep` - Sleep test page
- `http://localhost:7878/notfound` - Not found page

### Graceful Shutdown

Press `Ctrl-C` to gracefully shut down the server. The server will:
1. Stop accepting new connections
2. Wait for all ongoing requests to complete
3. Print final statistics
4. Exit cleanly

## Architecture

### Thread Pool
The server uses a custom thread pool to manage worker threads. The thread pool:
- Pre-allocates a fixed number of worker threads
- Distributes incoming jobs across available workers
- Ensures graceful shutdown by joining all worker threads

### Connection Handling
1. **Listener Thread**: Accepts incoming TCP connections
2. **Worker Threads**: Handle individual HTTP requests
3. **Reporter Thread**: Aggregates and reports statistics from all workers

### Statistics
The server tracks:
- Number of requests handled
- Response types and status codes
- Request metadata and timing information

## Dependencies

- **crossbeam** (0.8.4): Provides efficient multi-threading primitives
- **ctrlc** (3.4): Handles Ctrl-C signals for graceful shutdown
- **regex** (1.11.1): Regular expression support for request parsing

## Configuration

Default settings:
- **Address**: `localhost:7878`
- **Thread Pool Size**: 7 worker threads
- **HTML Files**: `hello.html`, `notfound.html`, `sleep.html`

## Future Enhancements

- Configurable port and address
- Adjustable thread pool size
- Additional HTTP methods support (POST, PUT, DELETE)
- HTTPS/TLS support
- Logging framework integration
- Configuration file support

## License

This project is educational and provided as-is.
