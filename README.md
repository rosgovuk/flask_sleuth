[![Build Status](https://travis-ci.org/davidcarboni/Flask-Sleuth.svg?branch=master)](https://travis-ci.org/davidcarboni/Flask-Sleuth)


# Flask Sleuth

Spring Cloud Sleuth logging implementation for Flask in Python 2/3.

## Purpose

Implement a Spring-style logging format to support interoperability and distributed tracing with Spring applications.

## What's in the box?

This project provides:

 * Logging in Spring Boot format using standard Python logging.
 * Distributed trace information (B3) in Spring Cloud Sleuth format.
 * `regex.regex.py` Regexes that document the log format, which is based on Spring Boot and Spring Cloud Sleuth.
 * `app.py` Python code to generate example log messages.

NB: this repo includes a copy of the [B3](https://github.com/davidcarboni/flask_b3) 
code in order to log tracing information.

## Usage

Careful design has gone in to making this code as simple as possible to use.
To add this to your project, you'll need to copy `flask_logging/__init__.py` and `b3/__init__.py`.
Once imported, you should be able to use completely standard Python logging:

    import logging
    import flask_logging
    app = Flask("My app")

That should be all you need.
Simply importing the `flask_logging` module calls `logging.basicConfig` and configures Python logging. 

NB `basicConfig` does nothing once the logging system is initialised.
If you currently call `basicConfig`, you'll need to think about how to work with this.

You can run `app.py` to see logging in action.

Here are the two intended use cases:

### Basic Spring Boot format

    logger = logging.getLogger("demo_logger")
    logger.setLevel(logging.DEBUG)
    logger.debug("Logging without tracing information")

### With B3 tracing information (Spring Cloud Sleuth format)

    logger = logging.getLogger("demo_logger")
    logger.setLevel(logging.DEBUG)
    b3.start_span()
    logger.debug("Logging with added tracing information")
    b3.end_span()

NB You'll likely want to call `start_span()` and `end_span()`
using `Flask.before_request()` and `Flask.after_request()`.
See the [flask_b3 README](https://github.com/davidcarboni/flask_b3/blob/master/README.md)
for details.
