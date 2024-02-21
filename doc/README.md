```shell
cd spug
git pull
cd spug_web
cnpm install
rm -rf build
npm run build
cd ../../
tar -czvf spug.tar.gz spug --exclude=spug_web/node_modules/* --exclude=spug-data/* --exclude=doc/*
tar -czvf spug.tar.gz spug --exclude=spug_web/node_modules/* --exclude=giraffe-data/* --exclude=doc/*
# tar -czvf spug.tar.gz docs  LICENSE  README.md  spug_api  spug_web
cd spug/docs/docker/
cp ../../../spug.tar.gz .
sudo docker build -f Dockerfile -t yiluxiangbei/spug:v1.0 .
sudo docker build --no-cache -f Dockerfile -t yiluxiangbei/spug:v1.0 .
sudo docker build -f Dockerfile.base -t yiluxiangbei/giraffe-base:v1.0 .
sudo docker build --no-cache -f Dockerfile.app -t yiluxiangbei/giraffe:v1.0 .

sudo docker push yiluxiangbei/spug:v1.0
sudo docker push yiluxiangbei/giraffe-base:v1.0
sudo docker push yiluxiangbei/giraffe:v1.0

cd ../../
sudo docker stop giraffe
sudo docker start giraffe
sudo docker rm giraffe
sudo docker run -d --restart=always --name=giraffe -p 8032:80 -v $(pwd)/giraffe-data:/data yiluxiangbei/giraffe:v1.0
sudo docker run -d --restart=always --name=giraffe -p 8032:80 -p 9001:9001 -p 9002:9002 -v $(pwd)/giraffe-data:/data yiluxiangbei/giraffe:v1.0
sudo docker exec giraffe init_giraffe admin giraffe.dev
rm -rf giraffe-data/spug/spug_web/*
cp -r spug_web/build/ giraffe-data/spug/spug_web/
cp -r spug_api/ giraffe-data/spug

sudo docker stop spug
sudo docker rm spug
sudo docker run -d --restart=always --name=spug -p 8032:80 -v $(pwd)/spug-data:/data yiluxiangbei/spug:v1.0
sudo docker exec spug init_spug admin spug.dev

sudo docker run -d --restart=always --name=spug -p 8032:80 -v $(pwd)/spug-data:/data registry.aliyuncs.com/openspug/spug
sudo docker exec spug init_spug admin spug.dev

sudo docker exec -it spug bash
sudo docker exec -it giraffe bash
mysqldump -uroot -p spug > spug.sql
mysql -uroot -p
create database giraffe;
mysql -uroot -p giraffe < spug.sql

sudo docker cp docs/docker/nginx.conf spug:/etc/nginx/nginx.conf
sudo docker stop spug
sudo docker start spug
sudo docker rm spug

Giraffe 长劲鹿
GIRAFFE

sudo yum install figlet
figlet "giraffe web terminal"

yum install -y yum-utils
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum install docker-ce docker-ce-cli containerd.io
systemctl start docker

docker pull registry.aliyuncs.com/openspug/spug

# 持久化存储启动命令：
# /spug 指的是映射本地的磁盘路径，也可以是其他目录，/data是容器内代码和数据初始化存储的路径
docker run -d --restart=always --name=spug -p 80:80 -v /spug:/data registry.aliyuncs.com/openspug/spug

# 如果你需要在spug内使用docker命令则需要添加额外的参数
docker run -d --restart=always --name=spug -p 80:80 -v /spug/:/data -v /var/run/docker.sock:/var/run/docker.sock -v /usr/bin/docker:/usr/bin/docker registry.aliyuncs.com/openspug/spug

docker exec spug init_spug admin spug.dev

sudo docker rmi $(docker images | grep "<none>" | awk "{print \$3}")
sudo docker rmi `docker images | grep none | awk '{print $3}'`
```

```
django 跨域
https://www.jianshu.com/p/16ce416555fe

https://spug.cc/docs/alarm-contact
https://spug.cc/docs/use-problem#use-dd
https://spug.cc/docs/batch-exec
https://spug.cc/docs/practice/
https://spug.cc/docs/conf-app
https://spug.cc/docs/deploy-config#global-env
https://spug.cc/docs/use-problem#clone
https://spug.cc/docs/deploy-config#transfer

https://my.oschina.net/u/5324949/blog/5510281
https://github.com/juicedata/juicefs
Kestra
https://github.com/kestra-io/kestra
https://kestra.io/
https://demo.kestra.io/ui/
https://github.com/coco-bigdata/Taier
http://narutogis.com/apps/JSViewer1.3/
https://github.com/DataLinkDC/dlink

https://confluence.atlassian.com/jirasoftwareserver0712/jira-software-server-7-12-documentation-959314519.html
https://docs.atlassian.com/software/jira/docs/api/REST/7.12.3/
https://doc.devpod.cn/jira/%E5%AE%9A%E4%B9%89%E9%A1%B9%E7%9B%AE-15237248.html
```

```shell
daphne --help
usage: daphne [-h] [-p PORT] [-b HOST] [--websocket_timeout WEBSOCKET_TIMEOUT]
              [--websocket_connect_timeout WEBSOCKET_CONNECT_TIMEOUT]
              [-u UNIX_SOCKET] [--fd FILE_DESCRIPTOR] [-e SOCKET_STRINGS]
              [-v VERBOSITY] [-t HTTP_TIMEOUT] [--access-log ACCESS_LOG]
              [--ping-interval PING_INTERVAL] [--ping-timeout PING_TIMEOUT]
              [--application-close-timeout APPLICATION_CLOSE_TIMEOUT]
              [--ws-protocol [WS_PROTOCOLS [WS_PROTOCOLS ...]]]
              [--asgi-protocol {asgi2,asgi3,auto}] [--root-path ROOT_PATH]
              [--proxy-headers] [--proxy-headers-host PROXY_HEADERS_HOST]
              [--proxy-headers-port PROXY_HEADERS_PORT] [-s SERVER_NAME]
              application

Django HTTP/WebSocket server

positional arguments:
  application           The application to dispatch to as
                        path.to.module:instance.path

optional arguments:
  -h, --help            show this help message and exit
  -p PORT, --port PORT  Port number to listen on
  -b HOST, --bind HOST  The host/address to bind to
  --websocket_timeout WEBSOCKET_TIMEOUT
                        Maximum time to allow a websocket to be connected. -1
                        for infinite.
  --websocket_connect_timeout WEBSOCKET_CONNECT_TIMEOUT
                        Maximum time to allow a connection to handshake. -1
                        for infinite
  -u UNIX_SOCKET, --unix-socket UNIX_SOCKET
                        Bind to a UNIX socket rather than a TCP host/port
  --fd FILE_DESCRIPTOR  Bind to a file descriptor rather than a TCP host/port
                        or named unix socket
  -e SOCKET_STRINGS, --endpoint SOCKET_STRINGS
                        Use raw server strings passed directly to twisted
  -v VERBOSITY, --verbosity VERBOSITY
                        How verbose to make the output
  -t HTTP_TIMEOUT, --http-timeout HTTP_TIMEOUT
                        How long to wait for worker before timing out HTTP
                        connections
  --access-log ACCESS_LOG
                        Where to write the access log (- for stdout, the
                        default for verbosity=1)
  --ping-interval PING_INTERVAL
                        The number of seconds a WebSocket must be idle before
                        a keepalive ping is sent
  --ping-timeout PING_TIMEOUT
                        The number of seconds before a WebSocket is closed if
                        no response to a keepalive ping
  --application-close-timeout APPLICATION_CLOSE_TIMEOUT
                        The number of seconds an ASGI application has to exit
                        after client disconnect before it is killed
  --ws-protocol [WS_PROTOCOLS [WS_PROTOCOLS ...]]
                        The WebSocket protocols you wish to support
  --asgi-protocol {asgi2,asgi3,auto}
                        The version of the ASGI protocol to use
  --root-path ROOT_PATH
                        The setting for the ASGI root_path variable
  --proxy-headers       Enable parsing and using of X-Forwarded-For and
                        X-Forwarded-Port headers and using that as the client
                        address
  --proxy-headers-host PROXY_HEADERS_HOST
                        Specify which header will be used for getting the host
                        part. Can be omitted, requires --proxy-headers to be
                        specified when passed. "X-Real-IP" (when passed by
                        your webserver) is a good candidate for this.
  --proxy-headers-port PROXY_HEADERS_PORT
                        Specify which header will be used for getting the port
                        part. Can be omitted, requires --proxy-headers to be
                        specified when passed.
  -s SERVER_NAME, --server-name SERVER_NAME
                        specify which value should be passed to response
                        header Server attribute

gunicorn --help
usage: gunicorn [OPTIONS] [APP_MODULE]

optional arguments:
  -h, --help            show this help message and exit
  -v, --version         show program's version number and exit
  -c CONFIG, --config CONFIG
                        The Gunicorn config file. [./gunicorn.conf.py]
  -b ADDRESS, --bind ADDRESS
                        The socket to bind. [['127.0.0.1:8000']]
  --backlog INT         The maximum number of pending connections. [2048]
  -w INT, --workers INT
                        The number of worker processes for handling requests.
                        [1]
  -k STRING, --worker-class STRING
                        The type of workers to use. [sync]
  --threads INT         The number of worker threads for handling requests.
                        [1]
  --worker-connections INT
                        The maximum number of simultaneous clients. [1000]
  --max-requests INT    The maximum number of requests a worker will process
                        before restarting. [0]
  --max-requests-jitter INT
                        The maximum jitter to add to the *max_requests*
                        setting. [0]
  -t INT, --timeout INT
                        Workers silent for more than this many seconds are
                        killed and restarted. [30]
  --graceful-timeout INT
                        Timeout for graceful workers restart. [30]
  --keep-alive INT      The number of seconds to wait for requests on a Keep-
                        Alive connection. [2]
  --limit-request-line INT
                        The maximum size of HTTP request line in bytes. [4094]
  --limit-request-fields INT
                        Limit the number of HTTP headers fields in a request.
                        [100]
  --limit-request-field_size INT
                        Limit the allowed size of an HTTP request header
                        field. [8190]
  --reload              Restart workers when code changes. [False]
  --reload-engine STRING
                        The implementation that should be used to power
                        :ref:`reload`. [auto]
  --reload-extra-file FILES
                        Extends :ref:`reload` option to also watch and reload
                        on additional files [[]]
  --spew                Install a trace function that spews every line
                        executed by the server. [False]
  --check-config        Check the configuration and exit. The exit status is 0
                        if the [False]
  --print-config        Print the configuration settings as fully resolved.
                        Implies :ref:`check-config`. [False]
  --preload             Load application code before the worker processes are
                        forked. [False]
  --no-sendfile         Disables the use of ``sendfile()``. [None]
  --reuse-port          Set the ``SO_REUSEPORT`` flag on the listening socket.
                        [False]
  --chdir CHDIR         Change directory to specified directory before loading
                        apps. [/]
  -D, --daemon          Daemonize the Gunicorn process. [False]
  -e ENV, --env ENV     Set environment variables in the execution
                        environment. [[]]
  -p FILE, --pid FILE   A filename to use for the PID file. [None]
  --worker-tmp-dir DIR  A directory to use for the worker heartbeat temporary
                        file. [None]
  -u USER, --user USER  Switch worker processes to run as this user. [0]
  -g GROUP, --group GROUP
                        Switch worker process to run as this group. [0]
  -m INT, --umask INT   A bit mask for the file mode on files written by
                        Gunicorn. [0]
  --initgroups          If true, set the worker process's group access list
                        with all of the [False]
  --forwarded-allow-ips STRING
                        Front-end's IPs from which allowed to handle set
                        secure headers. [127.0.0.1]
  --access-logfile FILE
                        The Access log file to write to. [None]
  --disable-redirect-access-to-syslog
                        Disable redirect access logs to syslog. [False]
  --access-logformat STRING
                        The access log format. [%(h)s %(l)s %(u)s %(t)s
                        "%(r)s" %(s)s %(b)s "%(f)s" "%(a)s"]
  --error-logfile FILE, --log-file FILE
                        The Error log file to write to. [-]
  --log-level LEVEL     The granularity of Error log outputs. [info]
  --capture-output      Redirect stdout/stderr to specified file in
                        :ref:`errorlog`. [False]
  --logger-class STRING
                        The logger you want to use to log events in Gunicorn.
                        [gunicorn.glogging.Logger]
  --log-config FILE     The log config file to use. [None]
  --log-syslog-to SYSLOG_ADDR
                        Address to send syslog messages. [udp://localhost:514]
  --log-syslog          Send *Gunicorn* logs to syslog. [False]
  --log-syslog-prefix SYSLOG_PREFIX
                        Makes Gunicorn use the parameter as program-name in
                        the syslog entries. [None]
  --log-syslog-facility SYSLOG_FACILITY
                        Syslog facility name [user]
  -R, --enable-stdio-inheritance
                        Enable stdio inheritance. [False]
  --statsd-host STATSD_ADDR
                        ``host:port`` of the statsd server to log to. [None]
  --dogstatsd-tags DOGSTATSD_TAGS
                        A comma-delimited list of datadog statsd (dogstatsd)
                        tags to append to []
  --statsd-prefix STATSD_PREFIX
                        Prefix to use when emitting statsd metrics (a trailing
                        ``.`` is added, []
  -n STRING, --name STRING
                        A base to use with setproctitle for process naming.
                        [None]
  --pythonpath STRING   A comma-separated list of directories to add to the
                        Python path. [None]
  --paste STRING, --paster STRING
                        Load a PasteDeploy config file. The argument may
                        contain a ``#`` [None]
  --proxy-protocol      Enable detect PROXY protocol (PROXY mode). [False]
  --proxy-allow-from PROXY_ALLOW_IPS
                        Front-end's IPs from which allowed accept proxy
                        requests (comma separate). [127.0.0.1]
  --keyfile FILE        SSL key file [None]
  --certfile FILE       SSL certificate file [None]
  --ssl-version SSL_VERSION
                        SSL version to use. [_SSLMethod.PROTOCOL_TLS]
  --cert-reqs CERT_REQS
                        Whether client certificate is required (see stdlib ssl
                        module's) [VerifyMode.CERT_NONE]
  --ca-certs FILE       CA certificates file [None]
  --suppress-ragged-eofs
                        Suppress ragged EOFs (see stdlib ssl module's) [True]
  --do-handshake-on-connect
                        Whether to perform SSL handshake on socket connect
                        (see stdlib ssl module's) [False]
  --ciphers CIPHERS     SSL Cipher suite to use, in the format of an OpenSSL
                        cipher list. [None]
  --paste-global CONF   Set a PasteDeploy global config variable in
                        ``key=value`` form. [[]]
  --strip-header-spaces
                        Strip spaces present between the header name and the
                        the ``:``. [False]

tar --help
Usage: tar [OPTION...] [FILE]...
GNU `tar' saves many files together into a single tape or disk archive, and can
restore individual files from the archive.

Examples:
  tar -cf archive.tar foo bar  # Create archive.tar from files foo and bar.
  tar -tvf archive.tar         # List all files in archive.tar verbosely.
  tar -xf archive.tar          # Extract all files from archive.tar.

 Main operation mode:

  -A, --catenate, --concatenate   append tar files to an archive
  -c, --create               create a new archive
  -d, --diff, --compare      find differences between archive and file system
      --delete               delete from the archive (not on mag tapes!)
  -r, --append               append files to the end of an archive
  -t, --list                 list the contents of an archive
      --test-label           test the archive volume label and exit
  -u, --update               only append files newer than copy in archive
  -x, --extract, --get       extract files from an archive

 Operation modifiers:

      --check-device         check device numbers when creating incremental
                             archives (default)
  -g, --listed-incremental=FILE   handle new GNU-format incremental backup
  -G, --incremental          handle old GNU-format incremental backup
      --ignore-failed-read   do not exit with nonzero on unreadable files
      --level=NUMBER         dump level for created listed-incremental archive
  -n, --seek                 archive is seekable
      --no-check-device      do not check device numbers when creating
                             incremental archives
      --no-seek              archive is not seekable
      --occurrence[=NUMBER]  process only the NUMBERth occurrence of each file
                             in the archive; this option is valid only in
                             conjunction with one of the subcommands --delete,
                             --diff, --extract or --list and when a list of
                             files is given either on the command line or via
                             the -T option; NUMBER defaults to 1
      --sparse-version=MAJOR[.MINOR]
                             set version of the sparse format to use (implies
                             --sparse)
  -S, --sparse               handle sparse files efficiently

 Overwrite control:

  -k, --keep-old-files       don't replace existing files when extracting,
                             treat them as errors
      --keep-directory-symlink   preserve existing symlinks to directories when
                             extracting
      --keep-newer-files     don't replace existing files that are newer than
                             their archive copies
      --no-overwrite-dir     preserve metadata of existing directories
      --overwrite            overwrite existing files when extracting
      --overwrite-dir        overwrite metadata of existing directories when
                             extracting (default)
      --recursive-unlink     empty hierarchies prior to extracting directory
      --remove-files         remove files after adding them to the archive
      --skip-old-files       don't replace existing files when extracting,
                             silently skip over them
  -U, --unlink-first         remove each file prior to extracting over it
  -W, --verify               attempt to verify the archive after writing it

 Select output stream:

      --ignore-command-error ignore exit codes of children
      --no-ignore-command-error   treat non-zero exit codes of children as
                             error
  -O, --to-stdout            extract files to standard output
      --to-command=COMMAND   pipe extracted files to another program

 Handling of file attributes:

      --atime-preserve[=METHOD]   preserve access times on dumped files, either
                             by restoring the times after reading
                             (METHOD='replace'; default) or by not setting the
                             times in the first place (METHOD='system')
      --delay-directory-restore   delay setting modification times and
                             permissions of extracted directories until the end
                             of extraction
      --group=NAME           force NAME as group for added files
      --mode=CHANGES         force (symbolic) mode CHANGES for added files
      --mtime=DATE-OR-FILE   set mtime for added files from DATE-OR-FILE
  -m, --touch                don't extract file modified time
      --no-delay-directory-restore
                             cancel the effect of --delay-directory-restore
                             option
      --no-same-owner        extract files as yourself (default for ordinary
                             users)
      --no-same-permissions  apply the user's umask when extracting permissions
                             from the archive (default for ordinary users)
      --numeric-owner        always use numbers for user/group names
      --owner=NAME           force NAME as owner for added files
  -p, --preserve-permissions, --same-permissions
                             extract information about file permissions
                             (default for superuser)
      --preserve             same as both -p and -s
      --same-owner           try extracting files with the same ownership as
                             exists in the archive (default for superuser)
  -s, --preserve-order, --same-order
                             member arguments are listed in the same order as
                             the files in the archive

 Handling of extended file attributes:

      --acls                 Enable the POSIX ACLs support
      --no-acls              Disable the POSIX ACLs support
      --no-selinux           Disable the SELinux context support
      --no-xattrs            Disable extended attributes support
      --selinux              Enable the SELinux context support
      --xattrs               Enable extended attributes support
      --xattrs-exclude=MASK  specify the exclude pattern for xattr keys
      --xattrs-include=MASK  specify the include pattern for xattr keys

 Device selection and switching:

  -f, --file=ARCHIVE         use archive file or device ARCHIVE
      --force-local          archive file is local even if it has a colon
  -F, --info-script=NAME, --new-volume-script=NAME
                             run script at end of each tape (implies -M)
  -L, --tape-length=NUMBER   change tape after writing NUMBER x 1024 bytes
  -M, --multi-volume         create/list/extract multi-volume archive
      --rmt-command=COMMAND  use given rmt COMMAND instead of rmt
      --rsh-command=COMMAND  use remote COMMAND instead of rsh
      --volno-file=FILE      use/update the volume number in FILE

 Device blocking:

  -b, --blocking-factor=BLOCKS   BLOCKS x 512 bytes per record
  -B, --read-full-records    reblock as we read (for 4.2BSD pipes)
  -i, --ignore-zeros         ignore zeroed blocks in archive (means EOF)
      --record-size=NUMBER   NUMBER of bytes per record, multiple of 512

 Archive format selection:

  -H, --format=FORMAT        create archive of the given format

 FORMAT is one of the following:

    gnu                      GNU tar 1.13.x format
    oldgnu                   GNU format as per tar <= 1.12
    pax                      POSIX 1003.1-2001 (pax) format
    posix                    same as pax
    ustar                    POSIX 1003.1-1988 (ustar) format
    v7                       old V7 tar format

      --old-archive, --portability
                             same as --format=v7
      --pax-option=keyword[[:]=value][,keyword[[:]=value]]...
                             control pax keywords
      --posix                same as --format=posix
  -V, --label=TEXT           create archive with volume name TEXT; at
                             list/extract time, use TEXT as a globbing pattern
                             for volume name

 Compression options:

  -a, --auto-compress        use archive suffix to determine the compression
                             program
  -I, --use-compress-program=PROG
                             filter through PROG (must accept -d)
  -j, --bzip2                filter the archive through bzip2
  -J, --xz                   filter the archive through xz
      --lzip                 filter the archive through lzip
      --lzma                 filter the archive through lzma
      --lzop
      --no-auto-compress     do not use archive suffix to determine the
                             compression program
  -z, --gzip, --gunzip, --ungzip   filter the archive through gzip
  -Z, --compress, --uncompress   filter the archive through compress

 Local file selection:

      --add-file=FILE        add given FILE to the archive (useful if its name
                             starts with a dash)
      --backup[=CONTROL]     backup before removal, choose version CONTROL
  -C, --directory=DIR        change to directory DIR
      --exclude=PATTERN      exclude files, given as a PATTERN
      --exclude-backups      exclude backup and lock files
      --exclude-caches       exclude contents of directories containing
                             CACHEDIR.TAG, except for the tag file itself
      --exclude-caches-all   exclude directories containing CACHEDIR.TAG
      --exclude-caches-under exclude everything under directories containing
                             CACHEDIR.TAG
      --exclude-tag=FILE     exclude contents of directories containing FILE,
                             except for FILE itself
      --exclude-tag-all=FILE exclude directories containing FILE
      --exclude-tag-under=FILE   exclude everything under directories
                             containing FILE
      --exclude-vcs          exclude version control system directories
  -h, --dereference          follow symlinks; archive and dump the files they
                             point to
      --hard-dereference     follow hard links; archive and dump the files they
                             refer to
  -K, --starting-file=MEMBER-NAME
                             begin at member MEMBER-NAME when reading the
                             archive
      --newer-mtime=DATE     compare date and time when data changed only
      --no-null              disable the effect of the previous --null option
      --no-recursion         avoid descending automatically in directories
      --no-unquote           do not unquote filenames read with -T
      --null                 -T reads null-terminated names, disable -C
  -N, --newer=DATE-OR-FILE, --after-date=DATE-OR-FILE
                             only store files newer than DATE-OR-FILE
      --one-file-system      stay in local file system when creating archive
  -P, --absolute-names       don't strip leading `/'s from file names
      --recursion            recurse into directories (default)
      --suffix=STRING        backup before removal, override usual suffix ('~'
                             unless overridden by environment variable
                             SIMPLE_BACKUP_SUFFIX)
  -T, --files-from=FILE      get names to extract or create from FILE
      --unquote              unquote filenames read with -T (default)
  -X, --exclude-from=FILE    exclude patterns listed in FILE

 File name transformations:

      --strip-components=NUMBER   strip NUMBER leading components from file
                             names on extraction
      --transform=EXPRESSION, --xform=EXPRESSION
                             use sed replace EXPRESSION to transform file
                             names

 File name matching options (affect both exclude and include patterns):

      --anchored             patterns match file name start
      --ignore-case          ignore case
      --no-anchored          patterns match after any `/' (default for
                             exclusion)
      --no-ignore-case       case sensitive matching (default)
      --no-wildcards         verbatim string matching
      --no-wildcards-match-slash   wildcards do not match `/'
      --wildcards            use wildcards (default)
      --wildcards-match-slash   wildcards match `/' (default for exclusion)

 Informative output:

      --checkpoint[=NUMBER]  display progress messages every NUMBERth record
                             (default 10)
      --checkpoint-action=ACTION   execute ACTION on each checkpoint
      --full-time            print file time to its full resolution
      --index-file=FILE      send verbose output to FILE
  -l, --check-links          print a message if not all links are dumped
      --no-quote-chars=STRING   disable quoting for characters from STRING
      --quote-chars=STRING   additionally quote characters from STRING
      --quoting-style=STYLE  set name quoting style; see below for valid STYLE
                             values
  -R, --block-number         show block number within archive with each message

      --show-defaults        show tar defaults
      --show-omitted-dirs    when listing or extracting, list each directory
                             that does not match search criteria
      --show-transformed-names, --show-stored-names
                             show file or archive names after transformation
      --totals[=SIGNAL]      print total bytes after processing the archive;
                             with an argument - print total bytes when this
                             SIGNAL is delivered; Allowed signals are: SIGHUP,
                             SIGQUIT, SIGINT, SIGUSR1 and SIGUSR2; the names
                             without SIG prefix are also accepted
      --utc                  print file modification times in UTC
  -v, --verbose              verbosely list files processed
      --warning=KEYWORD      warning control
  -w, --interactive, --confirmation
                             ask for confirmation for every action

 Compatibility options:

  -o                         when creating, same as --old-archive; when
                             extracting, same as --no-same-owner

 Other options:

  -?, --help                 give this help list
      --restrict             disable use of some potentially harmful options
      --usage                give a short usage message
      --version              print program version

Mandatory or optional arguments to long options are also mandatory or optional
for any corresponding short options.

The backup suffix is `~', unless set with --suffix or SIMPLE_BACKUP_SUFFIX.
The version control may be set with --backup or VERSION_CONTROL, values are:

  none, off       never make backups
  t, numbered     make numbered backups
  nil, existing   numbered if numbered backups exist, simple otherwise
  never, simple   always make simple backups

Valid arguments for the --quoting-style option are:

  literal
  shell
  shell-always
  c
  c-maybe
  escape
  locale
  clocale

*This* tar defaults to:
--format=gnu -f- -b20 --quoting-style=escape --rmt-command=/etc/rmt
--rsh-command=/usr/bin/ssh

Report bugs to <bug-tar@gnu.org>.
```