Два варианта логгирования, в терминал и в файл с ротацией (Есть также вариант просто в файл без ротации, но тут он не приведён)
Хранится в JSON формате, для удобного отображения в Grafana через Loki, либо в ELK.

```
<configuration>

    <!-- Консольный аппендер для вывода в терминал -->
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>
                {"timestamp": "%date{yyyy-MM-dd'T'HH:mm:ss.SSSZ}", 
                "level": "%level", 
                "thread": "%thread", 
                "logger": "%logger", 
                "message": "%message", 
                "exception": "%ex"}
            </pattern>
        </encoder>
    </appender>

    <!-- Корневой аппендер для записи в файл с ротацией -->
    <appender name="ROLLING" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>logs/myapp.log</file>

        <!-- Политика ротации логов: по времени -->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>logs/myapp-%d{yyyy-MM-dd}.log</fileNamePattern> <!-- Имя файлов с датой -->
            <maxHistory>30</maxHistory> <!-- Хранить логи за последние 30 дней -->
            <totalSizeCap>3GB</totalSizeCap> <!-- Максимальный размер всех файлов (например, 3GB) -->
        </rollingPolicy>

        <!-- Размер файла перед его ротацией -->
        <triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
            <maxFileSize>10MB</maxFileSize> <!-- Файл будет ротироваться при достижении 10MB -->
        </triggeringPolicy>

        <encoder>
            <!-- Форматирование логов в JSON для удобной обработки в ELK -->
            <pattern>
                {"timestamp": "%date{yyyy-MM-dd'T'HH:mm:ss.SSSZ}",
                "level": "%level",
                "thread": "%thread",
                "logger": "%logger",
                "message": "%message",
                "exception": "%ex"}
            </pattern>
        </encoder>
    </appender>

    <!-- Применение аппендера для корневого уровня логирования -->
    <root level="INFO">
        <appender-ref ref="ROLLING" />
        <appender-ref ref="CONSOLE" />
    </root>

</configuration>

```
