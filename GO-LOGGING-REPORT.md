
# Logging in Go, July 2021

[AwesomeGo](https://github.com/avelino/awesome-go) lists many loggers.  They have very
different capabilities and design philosophies.

None of the loggers support the 
[open-telemetry spec](https://github.com/open-telemetry/opentelemetry-proto/blob/main/opentelemetry/proto/logs/v1/logs.proto)

Logging in Go is a mess. [logur](https://github.com/logur/logur)'s README discusses this
mess and proposes a solution for library writers.

### Text loggers with a log object

* [kemba](https://github.com/clok/kemba): Colored logging with stackable prefixes.

### Leveled text loggers that use a log object:

* [distillog](https://github.com/amoghe/distillog): An `interface` with ties
  to other loggers. Very simple API.  This is the closest logger to liblogger.
* [glg](https://github.com/kpango/glg): Writes log levels to different places including HTTP and files. 
  Very big API, includes things like `func Cyan(str string) string` to color the log in cyan.
* [GLO](https://github.com/lajosbencz/glo): Suppors multiple destinations
  with handlers.  Can filter logs and format them.  Clean API.
* [go-logger (apsdehal)](https://github.com/apsdehal/go-logger): Supports output formats and colors.
* [gologger](https://github.com/sadlil/gologger): Supports colors and writing to console, files,
  and ElasticSearch.
* [log (aerogo)](https://github.com/aerogo/log): Write to multiple `io.Writer`s, two log levels.
* [log (teris-io)](https://github.com/teris-io/log): Inheritable key/Value fields for each log object
* [logger (azer)](https://github.com/azer/logger): colored logging control with the `LOG` environment
  variable. Key/Value tagging of logs.
* [logmatic](https://github.com/mborders/logmatic): Simple colored logging
* [Logo](https://github.com/mbndr/logo): Colored logs sent to multiple `io.Writer` "receivers"
* [go-hclog](https://github.com/hashicorp/go-hclog): Colors, filters, formatters, sinks.  From Hashicorp.
* [lager](https://github.com/cloudfoundry/lager): Formatter, redactor, multiple log sinks.
* [ozzo-log](https://github.com/go-ozzo/ozzo-log): Log to console, files, mail, and network.
  Log call stacks.  Filter logs.
* [stdlog](https://github.com/alexcesaro/log): standard (rich) logging interface with bufferend and
  unbuffered implementations.
* [zkits-logger](https://github.com/edoger/zkits-logger): Log to JSON and text with formats.

### Leveled text loggers without a log object

* [glob](https://github.com/golang/glog): From Google, almost an official logger for Go.
  Logging levels and destinations are set with command-line flags.  No logging object, uses globals.
* [go-log (pieterclaerhout)](https://github.com/pieterclaerhout/go-log): Easy logging of stack traces. Most of
  the controls are through setting global variables.
* [log (go-playground)](https://github.com/go-playground/log): Log routing based on log level; multiple handlers
  including console, syslog, http, email; key/value (string) tagging of logs; stack traces.
* [log15](https://github.com/inconshreveable/log15): Multiple and flexible log output handlers.
  Key/value tagging of log lines.  Logs are tagged with the caller, level, and time.
* [Logdump](https://github.com/ewwwwwqm/logdump): Enhances default `log` package with
  ability to route different levels to different places.
* [logutils](https://github.com/hashicorp/logutils): enhance the standard `log` with filters. 
  From Hashicorp.
* [mlog](https://github.com/jbrodriguez/mlog): file rotation and console logging.
* [seelog](https://github.com/cihub/seelog): Log configuration on the fly, loaded from XML.  Log
  to conole, file, rolling files, smtp, etc.  Log in JSON, XML, text.  Can pass logger to libraries
  that need one.

### Leveled text loggers with an optional log object

These loggers can pass around a log object but also provide logging at the global level
without a log object.

* [go-log (subchen)](https://github.com/subchen/go-log): Separate functions for creating `io.Writer`s 
  (similar to cronowriter).  Also provides an `interface` with leveled logging.
* [go-log (siddontang)](https://github.com/siddontang/go-log): Allows adding key/values to log lines.
* [Go-Log (ian-kent)](https://github.com/ian-kent/go-log): Supports multiple output "appender"s, including
  writing to Fluentd and rolling files.
* [gone/log](https://github.com/One-com/gone/tree/master/log): Flexible logger with coloring,
  formatting, and multiple writers
* [log (apex)](https://github.com/apex/log): Inheritable fields; logger embeddable in `Context`; handlers
  for many systems including Apex Logs, ElasticSearch, text, etc.
* [logrus](https://github.com/Sirupsen/logrus): key/value tagging, multiple `io.Writer`s, 
  multiple formatters including logstash, zalgo, GELF.
* [logging (sasbury)](https://github.com/sasbury/logging): Log to memory, rolling files, syslog, etc.
* [logxi](https://github.com/mgutz/logxi): key/value logging, JSON formatting options.  Controlled
  by environment varaibles.
* [xlog (xfxdev)](https://github.com/xfxdev/xlog): combine layouts and file writers writers
* [xlog (rs)](https://github.com/rs/xlog): Buffer logs through channels, discard logs if the channel
  is full.  Write to console, JSON, logstash, syslog.  Log URLs and user agents and request ids.

### Non-leveled loggers with a log object

* [log (go-kit)](https://github.com/go-kit/kit/tree/master/log): Inheritable key/value logs with
  formatted and JSON output adhering to a simple interface. Connectors to logrus, syslog, zap.

### Tracing

* [lunk](https://github.com/codahale/lunk): Create simple JSON logs that link contexts together
  with parent references
* [phosphor](https://github.com/monzo/phosphor): Distributed tracing system.  Not fully available.
* [zipkin-go](https://github.com/openzipkin/zipkin-go): Distributed tracing client.  Just spans,
  no logging, but tags are included.

### Log storage servers

* [logvoyage](https://github.com/logvoyage): Currently under development. Server receives logs.
  Frontend for viewing logs.

### Special purpose

* [httpretty](https://github.com/henvic/httpretty): Logs http request details to the console.
  JSON bodies will be printed.
* [jounald](https://github.com/ssgreg/journald): Sends key/value maps to systemd's
  Journal API.  Values can be `string`, `[]byte`, and whatever `fmt.Sprint` can handle.
* [logrusiowriter](https://github.com/cabify/logrusiowriter): Patch other uses of `log` to
  flow through to logrus.
* [logur](https://github.com/logur/logur): Generalized leveled log interface with provided
  implementations and adaptors to other libraries (hclog, go-kit log, logrus, zap, zerolog)
* [SQLDB-Logger](https://github.com/simukti/sqldb-logger): Wrapper for sql.Open() that 
  adds logging to DB calls.

### Support

* [cronowriter](https://github.com/utahta/go-cronowriter): Creates a `io.Writer`s for loggers with
  support for file naming based on the time/date.  Can hook to other loggers like Uber's zap
* [lumberjack](https://github.com/natefinch/lumberjack): provides an `io.WriteCloser` for logging
  to rolling files. Easy to integrate with `log`.
* [RollingWriter](https://github.com/arthurkiller/rollingWriter): provides an `io.Writer` for
  to rolling files.
* [go-spew](https://github.com/davecgh/go-spew): deep pretty printer for go structs, handles
  circular references.
* [yell](https://github.com/jfcg/yell): Logging library with helper functions for writing your
  own leveled logger.

### Structured loggers with a log object

* [zap](https://github.com/uber-go/zap): Log structured data with type-specific encoders
  quickly and any model with a reflective encoder. Inherited context (key/values).  Log 
  levels with HTTP hook to change log levels.  From Uber.
* [zerolog](https://github.com/rs/zerolog): Log structured data with type-specific encoders
  quickly and any model with a reflective encoder. Output as JSON, text, or CBOR.  Inherited
  context (key/values). Log levels.
* [onelog](https://pkg.go.dev/github.com/francoispqt/onelog): Log structured data with
  type-specific encoders quickly and any model with a reflective encoder.  JSON and console
  output.
* [phuslog](https://github.com/phuslu/log): Log structured data with type-specific encoders
  quickly and any model with a reflective encoder.  Multiple output streams including JSON,
  syslog, and console.  TSV logging.  Interface wrappers for Logr and grpclog.LoggerV2.
  RequestIds.

### Structured loggers without a log object

* [gomol](https://github.com/aphistic/gomol): Supports templates (how to format logs);
  attributes (extra bits added to log lines); and output to console, writers, JSON, and
  Loggly. 
* [Logex](https://github.com/chzyer/logex): Logs to Go's `log`, can log structs with JSON.

