Console
=======


MOAC developed based on the
`Ethereum <https://github.com/ethereum/go-ethereum>`__ project. It also
implements a ***javascript runtime environment*** (JSRE) that can be
used in either interactive (console) or non-interactive (script) mode.

MOAC Javascript console exposes the chain3 JavaScript Dapp API and the
admin API.

Interactive use: the JSRE REPL Console
--------------------------------------

启动MOAC的Console常用两种方式：
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

-  通过本地IPC连接

   ::

       ./moac attach /xx/xxx/xxxx.ipc

-  通过HTTP的RPC连接

   ::

       ./moac attach http://xxx.xxx.xxx.xxx:xxxx

   .. rubric:: 常用功能如下：
      :name: 常用功能如下

-  通过相对路径装载并运行javascript文件

   ::

       loadScript("./mctest.js")

// TODO 稍后更新
