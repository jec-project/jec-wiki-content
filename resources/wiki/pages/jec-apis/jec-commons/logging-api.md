# JEC Logging API

The JEC Commons Logging API provides developers to write log messages to a central place.

The Logging API captures information such as security failures, configuration errors, and/or bugs in the application.

## Working with Loggers

The `com.jec.commons.logging` package provides the logging capabilities via the [`Logger`](http://jecproject.org/docs/jec-commons/api/interfaces/logger.html) interface.

Developers must create `Logger` interface implementations to plug their frameworks to the JEC logging processor. The JEC Commons API ships with a default implentation, the [`ConsoleLogger`](http://jecproject.org/docs/jec-commons/api/classes/consolelogger.html) class, that writes logs in the standard output.

The best way for creating custom loggers is to extend the [`AbstractLogger`](http://jecproject.org/docs/jec-commons/api/classes/abstractlogger.html) abstract class.

The `Logger` interface provides methods that allow developers to specify logs by their criticity. For example, the following code will log an error message:

```typescript
    myLogger.error("process failed due to corrupted data");
```

## Log Level

The log levels define the severity of a message. The [`LogLevel`](http://jecproject.org/docs/jec-commons/api/enums/loglevel.html) enum is used to define which messages should be written to the log.

The following lists the log levels in ascending order:

- `TRACE`
- `DEBUG`
- `INFO`
- `WARN`
- `ERROR`

The `LogLevel` enum also defines two additional values:

- `ALWAYS` - outputs any kind of all logs, independently of their levels
- `OFF` - turns off outputs for all logs

`Logger` implementations only output messages for which log level are equal, or higher, to the logger's level.

You typically set the logger to the chosen level by using the `setLoglevel()` method:

```typescript
    myLogger.setLoglevel(LogLevel.WARN);
```

Thus, using the `LogLevel.WARN` level means that the following messages will never be sent to the output:

```typescript
    myLogger.trace("****************");
    myLogger.debug("process start");
    myLogger.info("object parsed");
    myLogger.debug("process complete");
    myLogger.trace("****************");
```

## Formatter

Developers can use formatters to configure the way outputs are displayed.
The [`LogFormatter`](http://jecproject.org/docs/jec-commons/api/interfaces/logformatter.html) interface defines the API that must be implemented to 
create message formatters for `Logger` objects.

The JEC Common API includes a standard Formatter: [`DefaultLogFormatter`](http://jecproject.org/docs/jec-commons/api/classes/defaultlogformatter.html).
This formatter will produce messages in the following format:

```shell
    [MM/DD/YY HH:mm:ss.SSS]<CUSTOM_CONTEXT> LEVEL: <MESSAGE>;
```