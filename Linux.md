# 常用命令

## curl

```shell
curl --help
Usage: curl [options...] <url>
     --abstract-unix-socket <path> Connect via abstract Unix domain socket
     --alt-svc <file name> Enable alt-svc with this cache file
     --anyauth       Pick any authentication method
 -a, --append        Append to target file when uploading
     --basic         Use HTTP Basic Authentication
     --cacert <file> CA certificate to verify peer against
     --capath <dir>  CA directory to verify peer against
 -E, --cert <certificate[:password]> Client certificate file and password
     --cert-status   Verify the status of the server certificate
     --cert-type <type> Certificate type (DER/PEM/ENG)
     --ciphers <list of ciphers> SSL ciphers to use
     --compressed    Request compressed response
     --compressed-ssh Enable SSH compression
 -K, --config <file> Read config from a file
     --connect-timeout <seconds> Maximum time allowed for connection
     --connect-to <HOST1:PORT1:HOST2:PORT2> Connect to host
 -C, --continue-at <offset> Resumed transfer offset
 -b, --cookie <data|filename> Send cookies from string/file
 -c, --cookie-jar <filename> Write cookies to <filename> after operation
     --create-dirs   Create necessary local directory hierarchy
     --crlf          Convert LF to CRLF in upload
     --crlfile <file> Get a CRL list in PEM format from the given file
 -d, --data <data>   HTTP POST data
     --data-ascii <data> HTTP POST ASCII data
     --data-binary <data> HTTP POST binary data
     --data-raw <data> HTTP POST data, '@' allowed
     --data-urlencode <data> HTTP POST data url encoded
     --delegation <LEVEL> GSS-API delegation permission
     --digest        Use HTTP Digest Authentication
 -q, --disable       Disable .curlrc
     --disable-eprt  Inhibit using EPRT or LPRT
     --disable-epsv  Inhibit using EPSV
     --disallow-username-in-url Disallow username in url
     --dns-interface <interface> Interface to use for DNS requests
     --dns-ipv4-addr <address> IPv4 address to use for DNS requests
     --dns-ipv6-addr <address> IPv6 address to use for DNS requests
     --dns-servers <addresses> DNS server addrs to use
     --doh-url <URL> Resolve host names over DOH
 -D, --dump-header <filename> Write the received headers to <filename>
     --egd-file <file> EGD socket path for random data
     --engine <name> Crypto engine to use
     --etag-compare <file> Pass an ETag from a file as a custom header
     --etag-save <file> Parse ETag from a request and save it to a file
     --expect100-timeout <seconds> How long to wait for 100-continue
 -f, --fail          Fail silently (no output at all) on HTTP errors
     --fail-early    Fail on first transfer error, do not continue
     --false-start   Enable TLS False Start
 -F, --form <name=content> Specify multipart MIME data
     --form-string <name=string> Specify multipart MIME data
     --ftp-account <data> Account data string
     --ftp-alternative-to-user <command> String to replace USER [name]
     --ftp-create-dirs Create the remote dirs if not present
     --ftp-method <method> Control CWD usage
     --ftp-pasv      Use PASV/EPSV instead of PORT
 -P, --ftp-port <address> Use PORT instead of PASV
     --ftp-pret      Send PRET before PASV
     --ftp-skip-pasv-ip Skip the IP address for PASV
     --ftp-ssl-ccc   Send CCC after authenticating
     --ftp-ssl-ccc-mode <active/passive> Set CCC mode
     --ftp-ssl-control Require SSL/TLS for FTP login, clear for transfer
 -G, --get           Put the post data in the URL and use GET
 -g, --globoff       Disable URL sequences and ranges using {} and []
     --happy-eyeballs-timeout-ms <milliseconds> Time for IPv6 before trying IPv4
     --haproxy-protocol Send HAProxy PROXY protocol v1 header
 -I, --head          Show document info only
 -H, --header <header/@file> Pass custom header(s) to server
 -h, --help          This help text
     --hostpubmd5 <md5> Acceptable MD5 hash of the host public key
     --http0.9       Allow HTTP 0.9 responses
 -0, --http1.0       Use HTTP 1.0
     --http1.1       Use HTTP 1.1
     --http2         Use HTTP 2
     --http2-prior-knowledge Use HTTP 2 without HTTP/1.1 Upgrade
     --http3         Use HTTP v3
     --ignore-content-length Ignore the size of the remote resource
 -i, --include       Include protocol response headers in the output # 打印出服务器回应的 HTTP 标头。
 -k, --insecure      Allow insecure server connections when using SSL
     --interface <name> Use network INTERFACE (or address)
 -4, --ipv4          Resolve names to IPv4 addresses
 -6, --ipv6          Resolve names to IPv6 addresses
 -j, --junk-session-cookies Ignore session cookies read from file
     --keepalive-time <seconds> Interval time for keepalive probes
     --key <key>     Private key file name
     --key-type <type> Private key file type (DER/PEM/ENG)
     --krb <level>   Enable Kerberos with security <level>
     --libcurl <file> Dump libcurl equivalent code of this command line
     --limit-rate <speed> Limit transfer speed to RATE
 -l, --list-only     List only mode
     --local-port <num/range> Force use of RANGE for local port numbers
 -L, --location      Follow redirects
     --location-trusted Like --location, and send auth to other hosts
     --login-options <options> Server login options
     --mail-auth <address> Originator address of the original email
     --mail-from <address> Mail from this address
     --mail-rcpt <address> Mail to this address
     --mail-rcpt-allowfails Allow RCPT TO command to fail for some recipients
 -M, --manual        Display the full manual
     --max-filesize <bytes> Maximum file size to download
     --max-redirs <num> Maximum number of redirects allowed
 -m, --max-time <seconds> Maximum time allowed for the transfer
     --metalink      Process given URLs as metalink XML file
     --negotiate     Use HTTP Negotiate (SPNEGO) authentication
 -n, --netrc         Must read .netrc for user name and password
     --netrc-file <filename> Specify FILE for netrc
     --netrc-optional Use either .netrc or URL
 -:, --next          Make next URL use its separate set of options
     --no-alpn       Disable the ALPN TLS extension
 -N, --no-buffer     Disable buffering of the output stream
     --no-keepalive  Disable TCP keepalive on the connection
     --no-npn        Disable the NPN TLS extension
     --no-progress-meter Do not show the progress meter
     --no-sessionid  Disable SSL session-ID reusing
     --noproxy <no-proxy-list> List of hosts which do not use proxy
     --ntlm          Use HTTP NTLM authentication
     --ntlm-wb       Use HTTP NTLM authentication with winbind
     --oauth2-bearer <token> OAuth 2 Bearer Token
 -o, --output <file> Write to file instead of stdout
 -Z, --parallel      Perform transfers in parallel
     --parallel-immediate Do not wait for multiplexing (with --parallel)
     --parallel-max  Maximum concurrency for parallel transfers
     --pass <phrase> Pass phrase for the private key
     --path-as-is    Do not squash .. sequences in URL path
     --pinnedpubkey <hashes> FILE/HASHES Public key to verify peer against
     --post301       Do not switch to GET after following a 301
     --post302       Do not switch to GET after following a 302
     --post303       Do not switch to GET after following a 303
     --preproxy [protocol://]host[:port] Use this proxy first
 -#, --progress-bar  Display transfer progress as a bar
     --proto <protocols> Enable/disable PROTOCOLS
     --proto-default <protocol> Use PROTOCOL for any URL missing a scheme
     --proto-redir <protocols> Enable/disable PROTOCOLS on redirect
 -x, --proxy [protocol://]host[:port] Use this proxy
     --proxy-anyauth Pick any proxy authentication method
     --proxy-basic   Use Basic authentication on the proxy
     --proxy-cacert <file> CA certificate to verify peer against for proxy
     --proxy-capath <dir> CA directory to verify peer against for proxy
     --proxy-cert <cert[:passwd]> Set client certificate for proxy
     --proxy-cert-type <type> Client certificate type for HTTPS proxy
     --proxy-ciphers <list> SSL ciphers to use for proxy
     --proxy-crlfile <file> Set a CRL list for proxy
     --proxy-digest  Use Digest authentication on the proxy
     --proxy-header <header/@file> Pass custom header(s) to proxy
     --proxy-insecure Do HTTPS proxy connections without verifying the proxy
     --proxy-key <key> Private key for HTTPS proxy
     --proxy-key-type <type> Private key file type for proxy
     --proxy-negotiate Use HTTP Negotiate (SPNEGO) authentication on the proxy
     --proxy-ntlm    Use NTLM authentication on the proxy
     --proxy-pass <phrase> Pass phrase for the private key for HTTPS proxy
     --proxy-pinnedpubkey <hashes> FILE/HASHES public key to verify proxy with
     --proxy-service-name <name> SPNEGO proxy service name
     --proxy-ssl-allow-beast Allow security flaw for interop for HTTPS proxy
     --proxy-tls13-ciphers <ciphersuite list> TLS 1.3 proxy cipher suites
     --proxy-tlsauthtype <type> TLS authentication type for HTTPS proxy
     --proxy-tlspassword <string> TLS password for HTTPS proxy
     --proxy-tlsuser <name> TLS username for HTTPS proxy
     --proxy-tlsv1   Use TLSv1 for HTTPS proxy
 -U, --proxy-user <user:password> Proxy user and password
     --proxy1.0 <host[:port]> Use HTTP/1.0 proxy on given port
 -p, --proxytunnel   Operate through an HTTP proxy tunnel (using CONNECT)
     --pubkey <key>  SSH Public key file name
 -Q, --quote         Send command(s) to server before transfer
     --random-file <file> File for reading random data from
 -r, --range <range> Retrieve only the bytes within RANGE
     --raw           Do HTTP "raw"; no transfer decoding
 -e, --referer <URL> Referrer URL
 -J, --remote-header-name Use the header-provided filename
 -O, --remote-name   Write output to a file named as the remote file
     --remote-name-all Use the remote file name for all URLs
 -R, --remote-time   Set the remote file's time on the local output
 -X, --request <command> Specify request command to use # 参数指定 HTTP 请求的方法。POST GET
     --request-target Specify the target for this request
     --resolve <host:port:addr[,addr]...> Resolve the host+port to this address
     --retry <num>   Retry request if transient problems occur
     --retry-all-errors Retry all errors (use with --retry)
     --retry-connrefused Retry on connection refused (use with --retry)
     --retry-delay <seconds> Wait time between retries
     --retry-max-time <seconds> Retry only within this period
     --sasl-authzid <identity> Identity for SASL PLAIN authentication
     --sasl-ir       Enable initial response in SASL authentication
     --service-name <name> SPNEGO service name
 -S, --show-error    Show error even when -s is used
 -s, --silent        Silent mode
     --socks4 <host[:port]> SOCKS4 proxy on given host + port
     --socks4a <host[:port]> SOCKS4a proxy on given host + port
     --socks5 <host[:port]> SOCKS5 proxy on given host + port
     --socks5-basic  Enable username/password auth for SOCKS5 proxies
     --socks5-gssapi Enable GSS-API auth for SOCKS5 proxies
     --socks5-gssapi-nec Compatibility with NEC SOCKS5 server
     --socks5-gssapi-service <name> SOCKS5 proxy service name for GSS-API
     --socks5-hostname <host[:port]> SOCKS5 proxy, pass host name to proxy
 -Y, --speed-limit <speed> Stop transfers slower than this
 -y, --speed-time <seconds> Trigger 'speed-limit' abort after this time
     --ssl           Try SSL/TLS
     --ssl-allow-beast Allow security flaw to improve interop
     --ssl-no-revoke Disable cert revocation checks (Schannel)
     --ssl-reqd      Require SSL/TLS
     --ssl-revoke-best-effort Ignore missing/offline cert CRL dist points
 -2, --sslv2         Use SSLv2
 -3, --sslv3         Use SSLv3
     --stderr        Where to redirect stderr
     --styled-output Enable styled output for HTTP headers
     --suppress-connect-headers Suppress proxy CONNECT response headers
     --tcp-fastopen  Use TCP Fast Open
     --tcp-nodelay   Use the TCP_NODELAY option
 -t, --telnet-option <opt=val> Set telnet option
     --tftp-blksize <value> Set TFTP BLKSIZE option
     --tftp-no-options Do not send any TFTP options
 -z, --time-cond <time> Transfer based on a time condition
     --tls-max <VERSION> Set maximum allowed TLS version
     --tls13-ciphers <ciphersuite list> TLS 1.3 cipher suites to use
     --tlsauthtype <type> TLS authentication type
     --tlspassword   TLS password
     --tlsuser <name> TLS user name
 -1, --tlsv1         Use TLSv1.0 or greater
     --tlsv1.0       Use TLSv1.0 or greater
     --tlsv1.1       Use TLSv1.1 or greater
     --tlsv1.2       Use TLSv1.2 or greater
     --tlsv1.3       Use TLSv1.3 or greater
     --tr-encoding   Request compressed transfer encoding
     --trace <file>  Write a debug trace to FILE
     --trace-ascii <file> Like --trace, but without hex output
     --trace-time    Add time stamps to trace/verbose output
     --unix-socket <path> Connect through this Unix domain socket
 -T, --upload-file <file> Transfer local FILE to destination
     --url <url>     URL to work with
 -B, --use-ascii     Use ASCII/text transfer
 -u, --user <user:password> Server user and password
 -A, --user-agent <name> Send User-Agent <name> to server
 -v, --verbose       Make the operation more talkative
 -V, --version       Show version number and quit
 -w, --write-out <format> Use output FORMAT after completion
     --xattr         Store metadata in extended file attributes
```



```shell
# POST 数据到某个URL
curl -d "page=1&limit=10&label=0&name=" -X POST https://xm.kadianshipin.com/api//fingertip/video/long

```



## rsync

```shell
rsync -av --progress ./ user@ip:/xxx/xxx/xxx/path
```

## awk

```shell
awk -help
Usage: awk [POSIX or GNU style options] -f progfile [--] file ...
Usage: awk [POSIX or GNU style options] [--] 'program' file ...
POSIX options:		GNU long options: (standard)
	-f progfile		--file=progfile
	-F fs			--field-separator=fs # 分隔符 比如nginx的access log ： awk -F \" '{print $6}' 按照双引号为分隔符分割
	-v var=val		--assign=var=val
Short options:		GNU long options: (extensions)
	-b			--characters-as-bytes
	-c			--traditional
	-C			--copyright
	-d[file]		--dump-variables[=file]
	-e 'program-text'	--source='program-text'
	-E file			--exec=file
	-g			--gen-pot
	-h			--help
	-L [fatal]		--lint[=fatal]
	-n			--non-decimal-data
	-N			--use-lc-numeric
	-O			--optimize
	-p[file]		--profile[=file]
	-P			--posix
	-r			--re-interval
	-S			--sandbox
	-t			--lint-old
	-V			--version

To report bugs, see node `Bugs' in `gawk.info', which is
section `Reporting Problems and Bugs' in the printed version.

gawk is a pattern scanning and processing language.
By default it reads standard input and writes standard output.

Examples:
	gawk '{ sum += $1 }; END { print sum }' file
	gawk -F: '{ print $1 }' /etc/passwd
	
	
```

### 小练习

1. Nginx的web服务器日志 access log文件，需要从统计日志中统计有哪些类型的设备终端和浏览器访问了该网站

   ````shell
   #1. 先解析出来所有的 user-agent
   # 处理该文件的每一行，指定 " 为分隔符，只输出第6个文本域
   cat /www/wwwlogs/local.web.access.log | awk -F \" '{print $6}' > ua.txt  
   
   ```
   Mozilla/5.0 (Linux; U; Android 9; zh-CN; MI 8 Build/PKQ1.180729.001) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/57.0.2987.108 UCBrowser/12.8.9.1069 Mobi
   le Safari/537.36
   Mozilla/5.0 (Linux; U; Android 9; zh-CN; MI 8 Build/PKQ1.180729.001) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/57.0.2987.108 UCBrowser/12.8.9.1069 Mobi
   le Safari/537.36
   Mozilla/5.0 (Linux; U; Android 9; zh-CN; MI 8 Build/PKQ1.180729.001) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/57.0.2987.108 UCBrowser/12.8.9.1069 Mobi
   le Safari/537.36
   Mozilla/5.0 (Linux; U; Android 9; zh-CN; MI 8 Build/PKQ1.180729.001) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/57.0.2987.108 UCBrowser/12.8.9.1069 Mobi
   le Safari/537.36
   Mozilla/5.0 (Linux; U; Android 9; zh-CN; MI 8 Build/PKQ1.180729.001) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/57.0.2987.108 UCBrowser/12.8.9.1069 Mobi
   le Safari/537.36
   Mozilla/5.0 (Linux; U; Android 9; zh-CN; MI 8 Build/PKQ1.180729.001) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/57.0.2987.108 UCBrowser/12.8.9.1069 Mobi
   le Safari/537.36
   Dalvik/2.1.0 (Linux; U; Android 9.0; ZTE BA520 Build/MRA58K)
   ```
   
   #2. 将user-agent 做唯一值处理
   sort ua.txt | uniq -u > ua1.txt
   
   # 也可以直接通过管道命令：
   cat  /www/wwwlogs/local.web.access.log| awk -F \" '{print $6}' | sort | uniq -c | sort -nr
   ````

   

# supervisor

## 安装

```shell
(base) [root@localhost home]# conda activate py3-7
(py3-7) [root@localhost home]#
(py3-7) [root@localhost home]# pip install supervisor
Collecting supervisor
  Downloading supervisor-4.2.4-py2.py3-none-any.whl (749 kB)
     |████████████████████████████████| 749 kB 36 kB/s
Requirement already satisfied: setuptools in /root/anaconda3/envs/py3-7/lib/python3.7/site-packages (from supervisor) (58.0.4)
Installing collected packages: supervisor
Successfully installed supervisor-4.2.4
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv
```

```shell
(py3-7) [root@localhost home]# echo_supervisord_conf
; Sample supervisor config file.
;
; For more information on the config file, please see:
; http://supervisord.org/configuration.html
;
; Notes:
;  - Shell expansion ("~" or "$HOME") is not supported.  Environment
;    variables can be expanded using this syntax: "%(ENV_HOME)s".
;  - Quotes around values are not supported, except in the case of
;    the environment= options as shown below.
;  - Comments must have a leading space: "a=b ;comment" not "a=b;comment".
;  - Command will be truncated if it looks like a config file comment, e.g.
;    "command=bash -c 'foo ; bar'" will truncate to "command=bash -c 'foo ".
;
; Warning:
;  Paths throughout this example file use /tmp because it is available on most
;  systems.  You will likely need to change these to locations more appropriate
;  for your system.  Some systems periodically delete older files in /tmp.
;  Notably, if the socket file defined in the [unix_http_server] section below
;  is deleted, supervisorctl will be unable to connect to supervisord.

[unix_http_server]
file=/tmp/supervisor.sock   ; the path to the socket file
;chmod=0700                 ; socket file mode (default 0700)
;chown=nobody:nogroup       ; socket file uid:gid owner
;username=user              ; default is no username (open server)
;password=123               ; default is no password (open server)

; Security Warning:
;  The inet HTTP server is not enabled by default.  The inet HTTP server is
;  enabled by uncommenting the [inet_http_server] section below.  The inet
;  HTTP server is intended for use within a trusted environment only.  It
;  should only be bound to localhost or only accessible from within an
;  isolated, trusted network.  The inet HTTP server does not support any
;  form of encryption.  The inet HTTP server does not use authentication
;  by default (see the username= and password= options to add authentication).
;  Never expose the inet HTTP server to the public internet.

;[inet_http_server]         ; inet (TCP) server disabled by default
;port=127.0.0.1:9001        ; ip_address:port specifier, *:port for all iface
;username=user              ; default is no username (open server)
;password=123               ; default is no password (open server)

[supervisord]
logfile=/tmp/supervisord.log ; main log file; default $CWD/supervisord.log
logfile_maxbytes=50MB        ; max main logfile bytes b4 rotation; default 50MB
logfile_backups=10           ; # of main logfile backups; 0 means none, default 10
loglevel=info                ; log level; default info; others: debug,warn,trace
pidfile=/tmp/supervisord.pid ; supervisord pidfile; default supervisord.pid
nodaemon=false               ; start in foreground if true; default false
silent=false                 ; no logs to stdout if true; default false
minfds=1024                  ; min. avail startup file descriptors; default 1024
minprocs=200                 ; min. avail process descriptors;default 200
;umask=022                   ; process file creation umask; default 022
;user=supervisord            ; setuid to this UNIX account at startup; recommended if root
;identifier=supervisor       ; supervisord identifier, default is 'supervisor'
;directory=/tmp              ; default is not to cd during start
;nocleanup=true              ; don't clean up tempfiles at start; default false
;childlogdir=/tmp            ; 'AUTO' child log dir, default $TEMP
;environment=KEY="value"     ; key value pairs to add to environment
;strip_ansi=false            ; strip ansi escape codes in logs; def. false

; The rpcinterface:supervisor section must remain in the config file for
; RPC (supervisorctl/web interface) to work.  Additional interfaces may be
; added by defining them in separate [rpcinterface:x] sections.

[rpcinterface:supervisor]
supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface

; The supervisorctl section configures how supervisorctl will connect to
; supervisord.  configure it match the settings in either the unix_http_server
; or inet_http_server section.

[supervisorctl]
serverurl=unix:///tmp/supervisor.sock ; use a unix:// URL  for a unix socket
;serverurl=http://127.0.0.1:9001 ; use an http:// url to specify an inet socket
;username=chris              ; should be same as in [*_http_server] if set
;password=123                ; should be same as in [*_http_server] if set
;prompt=mysupervisor         ; cmd line prompt (default "supervisor")
;history_file=~/.sc_history  ; use readline history if available

; The sample program section below shows all possible program subsection values.
; Create one or more 'real' program: sections to be able to control them under
; supervisor.

;[program:theprogramname]
;command=/bin/cat              ; the program (relative uses PATH, can take args)
;process_name=%(program_name)s ; process_name expr (default %(program_name)s)
;numprocs=1                    ; number of processes copies to start (def 1)
;directory=/tmp                ; directory to cwd to before exec (def no cwd)
;umask=022                     ; umask for process (default None)
;priority=999                  ; the relative start priority (default 999)
;autostart=true                ; start at supervisord start (default: true)
;startsecs=1                   ; # of secs prog must stay up to be running (def. 1)
;startretries=3                ; max # of serial start failures when starting (default 3)
;autorestart=unexpected        ; when to restart if exited after running (def: unexpected)
;exitcodes=0                   ; 'expected' exit codes used with autorestart (default 0)
;stopsignal=QUIT               ; signal used to kill process (default TERM)
;stopwaitsecs=10               ; max num secs to wait b4 SIGKILL (default 10)
;stopasgroup=false             ; send stop signal to the UNIX process group (default false)
;killasgroup=false             ; SIGKILL the UNIX process group (def false)
;user=chrism                   ; setuid to this UNIX account to run the program
;redirect_stderr=true          ; redirect proc stderr to stdout (default false)
;stdout_logfile=/a/path        ; stdout log path, NONE for none; default AUTO
;stdout_logfile_maxbytes=1MB   ; max # logfile bytes b4 rotation (default 50MB)
;stdout_logfile_backups=10     ; # of stdout logfile backups (0 means none, default 10)
;stdout_capture_maxbytes=1MB   ; number of bytes in 'capturemode' (default 0)
;stdout_events_enabled=false   ; emit events on stdout writes (default false)
;stdout_syslog=false           ; send stdout to syslog with process name (default false)
;stderr_logfile=/a/path        ; stderr log path, NONE for none; default AUTO
;stderr_logfile_maxbytes=1MB   ; max # logfile bytes b4 rotation (default 50MB)
;stderr_logfile_backups=10     ; # of stderr logfile backups (0 means none, default 10)
;stderr_capture_maxbytes=1MB   ; number of bytes in 'capturemode' (default 0)
;stderr_events_enabled=false   ; emit events on stderr writes (default false)
;stderr_syslog=false           ; send stderr to syslog with process name (default false)
;environment=A="1",B="2"       ; process environment additions (def no adds)
;serverurl=AUTO                ; override serverurl computation (childutils)

; The sample eventlistener section below shows all possible eventlistener
; subsection values.  Create one or more 'real' eventlistener: sections to be
; able to handle event notifications sent by supervisord.

;[eventlistener:theeventlistenername]
;command=/bin/eventlistener    ; the program (relative uses PATH, can take args)
;process_name=%(program_name)s ; process_name expr (default %(program_name)s)
;numprocs=1                    ; number of processes copies to start (def 1)
;events=EVENT                  ; event notif. types to subscribe to (req'd)
;buffer_size=10                ; event buffer queue size (default 10)
;directory=/tmp                ; directory to cwd to before exec (def no cwd)
;umask=022                     ; umask for process (default None)
;priority=-1                   ; the relative start priority (default -1)
;autostart=true                ; start at supervisord start (default: true)
;startsecs=1                   ; # of secs prog must stay up to be running (def. 1)
;startretries=3                ; max # of serial start failures when starting (default 3)
;autorestart=unexpected        ; autorestart if exited after running (def: unexpected)
;exitcodes=0                   ; 'expected' exit codes used with autorestart (default 0)
;stopsignal=QUIT               ; signal used to kill process (default TERM)
;stopwaitsecs=10               ; max num secs to wait b4 SIGKILL (default 10)
;stopasgroup=false             ; send stop signal to the UNIX process group (default false)
;killasgroup=false             ; SIGKILL the UNIX process group (def false)
;user=chrism                   ; setuid to this UNIX account to run the program
;redirect_stderr=false         ; redirect_stderr=true is not allowed for eventlisteners
;stdout_logfile=/a/path        ; stdout log path, NONE for none; default AUTO
;stdout_logfile_maxbytes=1MB   ; max # logfile bytes b4 rotation (default 50MB)
;stdout_logfile_backups=10     ; # of stdout logfile backups (0 means none, default 10)
;stdout_events_enabled=false   ; emit events on stdout writes (default false)
;stdout_syslog=false           ; send stdout to syslog with process name (default false)
;stderr_logfile=/a/path        ; stderr log path, NONE for none; default AUTO
;stderr_logfile_maxbytes=1MB   ; max # logfile bytes b4 rotation (default 50MB)
;stderr_logfile_backups=10     ; # of stderr logfile backups (0 means none, default 10)
;stderr_events_enabled=false   ; emit events on stderr writes (default false)
;stderr_syslog=false           ; send stderr to syslog with process name (default false)
;environment=A="1",B="2"       ; process environment additions
;serverurl=AUTO                ; override serverurl computation (childutils)

; The sample group section below shows all possible group values.  Create one
; or more 'real' group: sections to create "heterogeneous" process groups.

;[group:thegroupname]
;programs=progname1,progname2  ; each refers to 'x' in [program:x] definitions
;priority=999                  ; the relative start priority (default 999)

; The [include] section can just contain the "files" setting.  This
; setting can list multiple files (separated by whitespace or
; newlines).  It can also contain wildcards.  The filenames are
; interpreted as relative to this file.  Included files *cannot*
; include files themselves.

;[include]
;files = relative/directory/*.ini
```



## 配置

```shell
```





# ELK

## 安装

## Filebeat

### 开启mysql模块

https://help.aliyun.com/document_detail/170479.html

```shell
> filebeat modules enable mysql
> vim /etc/filebeat/modules.d/mysql.yml

var.paths: ["/www/server/data/mysql-slow.log","/data2/www/server/mysqldata/data3308/mysql-slow.log"] # 打开

> service filebeat restart
```



### 启动命令

```shell
systemctl restart elasticsearch.service
systemctl restart filebeat.service
```



## kibana-keystore

```shell
cd /usr/share/kibana/bin/
./bin/kibana-keystore list

# Unable to create actions client because the Encrypted Saved Objects plugin is missing encryption key. Please set xpack.encryptedSavedObjects.encryptionKey in the kibana.yml or use the bin/kibana-encryption-keys command.
# 解决这个问题，就是在kibana.yml 文件新增 xpack.encryptedSavedObjects.encryptionKey。kibana为了方便，提供了 kibana-keystore 工具，可以自动添加，add后 不需要在kibana.yml 中再次添加。
./bin/kibana-keystore add xpack.encryptedSavedObjects.encryptionKey
Enter value for xpack.encryptedSavedObjects.encryptionKey: ********************************

E1XL3pvgwYyL47p7YKZwj4aNvxoSSiuE

./bin/kibana-keystore add  elasticsearch.username
Enter value for elasticsearch.username: ******* 

./bin/kibana-keystore add  elasticsearch.password
Enter value for elasticsearch.password: *******

./bin/kibana-keystore list
xpack.encryptedSavedObjects.encryptionKey
elasticsearch.username
elasticsearch.password
```



## 安全

https://www.elastic.co/guide/en/kibana/current/using-kibana-with-security.html

https://www.elastic.co/guide/en/kibana/7.9/alert-action-settings-kb.html#general-alert-action-settings



## 常见问题

1. Unable to create actions client because the Encrypted Saved Objects plugin is missing encryption key. Please set xpack.encryptedSavedObjects.encryptionKey in the kibana.yml or use the bin/kibana-encryption-keys command.

2. exception: Security must be explicitly enabled when using a [basic] license. Enable security by setting [xpack.security.enabled] to [true] in the elasticsearch.yml file and restart the node. (500)

3. License is not available.（es的用户名密码校验不通过）

   ```json
   {"statusCode":503,"error":"Service Unavailable","message":"License is not available."}
   ```

   

4. 日志

   ```shell
    tail -f /var/log/kibana/kibana.log
   {"type":"log","@timestamp":"2021-11-17T16:19:16+08:00","tags":["warning","plugins","licensing"],"pid":2935,"message":"License information could not be obtained from Elasticsearch due to ConnectionError: connect ECONNREFUSED 127.0.0.1:9200 error"}
   ```

   

5. xx

6. xx

# SSH免密登录



SSH免密登录一般可以采用下面三种方式，看个人习惯。

## ① ssh-copy-id

```shell
ssh-copy-id -i ~/.ssh/id_rsa.pub root@xx.xx.xx.xx  # 然后输入对应的密码即可。

# /usr/bin/ssh-copy-id: ERROR: failed to open ID file '/root/.ssh/id_rsa.pub': 没有那个文件或目录 需要生成文件

(base) [root@localhost ~]# cd ~/.ssh/
(base) [root@localhost .ssh]# ll
总用量 4
-rw-------. 1 root root 416 11月 24 17:27 authorized_keys
(base) [root@localhost .ssh]#
(base) [root@localhost .ssh]# ssh-keygen -o
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:IabxS8gmcE42LJ3gS2XrhAF1pH5XWGdRc8C6cpin8fk root@localhost
The key's randomart image is:
+---[RSA 3072]----+
|+o.+o   . +++..  |
|.+=+.  o o  .o   |
|o+Xo. + o  .     |
|.O+o * o ..      |
| .+.* + So .     |
|   + o .= +      |
|      .  B .     |
|        . o      |
|           .E    |
+----[SHA256]-----+
(base) [root@localhost .ssh]# ll
总用量 12
-rw-------. 1 root root  416 11月 24 17:27 authorized_keys
-rw-------. 1 root root 2602 12月  1 18:13 id_rsa
-rw-r--r--. 1 root root  568 12月  1 18:13 id_rsa.pub


```

## ② scp

```shell
# 通过 scp 命令直接将该文件远程复制过去，使用这种方式需要注意，如果你之前已经配置了其它服务器上的密钥，这是使用这种方法，就会覆盖掉你原来的密钥，这时候是不建议使用这种方式的，如果你是先将该文件复制到服务器上的一个目录下，然后在使用追加的方式，将密钥追加到 authorized_keys 也是完全 OK 的。
scp -p ~/.ssh/id_rsa.pub root@:/root/.ssh/authorized_keys
```

## ③ 纯手工复制

通过手工复制。将本地 id_rsa.pub 文件的内容拷贝至远程服务器的 ~/.ssh/authorized_keys 文件中也完全可以的。

先使用 cat 命令查看当前的公钥，然后复制，在到目标服务器上去粘贴。

### 别名配置

```shell
vim ~/.ssh/config
# 添加以下内容
Host server_name
        HostName xxx.xxx.xxx.xxx
        User root
        Port 22
        
# 保存退出

ssh server_name # 即可直接登入服务器。方便很多。
```









# 常见问题

## ssh 登录会话保持时间太短解决方法：

```shell
vim /etc/ssh/sshd_config

ClientAliveInterval 60
ClientAliveCountMax 3

service sshd reload 重启服务
```

## 内网IP配置

```shell
# ip addr 查看网卡
vim /etc/sysconfig/network-scripts/ifcfg-enp5s0f1

TYPE=Ethernet
BOOTPROTO=dhcp # 改为 static
DEFROUTE=yes
PEERDNS=yes
PEERROUTES=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_PEERDNS=yes
IPV6_PEERROUTES=yes
IPV6_FAILURE_FATAL=no
NAME=enp5s0f1
UUID=91a7771b-9c71-418f-bf3e-eede0d1f54c6
DEVICE=enp5s0f1
ONBOOT=no  # yes

IPADDR=192.168.1.192 #内网IP 地址
METMASK=255.255.255.0 
GATEWAY=192.168.1.1 

# 最后一步 重启网卡
service network restart 
```



# BT

## 安装

参考文档：https://www.bt.cn/bbs/thread-19376-1-1.html

```shell
# yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh

```

