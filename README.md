Forked from TheZeroSlave/zapsentry

Improvements:
- go 1.13
- sentry-go v0.10.0
- converting logger name to sentry logger
- filtering k8s logger dependency - github.com/go-logr/zapr

# Sentry client for zap logger

Integration of sentry client into zap.Logger is pretty simple:
```golang
func modifyToSentryLogger(log *zap.Logger, DSN string) *zap.Logger {
	cfg := zapsentry.Configuration{
		Level: zapcore.ErrorLevel, //when to send message to sentry
		Tags: map[string]string{
			"component": "system",
		},
	}
	core, err := zapsentry.NewCore(cfg, zapsentry.NewSentryClientFromDSN(DSN))
	//in case of err it will return noop core. so we can safely attach it
	if err != nil {
		log.Warn("failed to init zap", zap.Error(err))
	}
	return zapsentry.AttachCoreToLogger(core, log)
}
```
