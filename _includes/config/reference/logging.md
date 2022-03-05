log.root | The root folder where logs are written to. | log
logger | Logger instance to use. | Logger.new($stderr)
logger.formatter | Logger Formatter to use. See [Formatter](https://ruby-doc.org/stdlib-2.7.1/libdoc/logger/rdoc/Logger/Formatter.html) for interface. | [Ufo::Logger::Formatter](https://github.com/boltops-tools/ufo/blob/master/lib/ufo/logger/formatter.rb)
logger.level | Logger level | info
logs.filter_pattern | nil | Default filter pattern to use. The CLI option overrides this setting. Example: `config.logs.filter_pattern = '- "HealthChecker"'`. Note the `-` minus sign rejects patterns. See: [AWS Docs: CloudWatch Logs Filter and Syntax Pattern](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/FilterAndPatternSyntax.html).