### Promise基本规范

* 1、Promise存在三个状态：pending、fulfilled、rejectted

* 2、Pending为初始态，并可以转换为fulfilled和rejected

* 3、成功时，不可转为其他状态，且必须有一个不可改变的值（value）

* 4、失败时，不可转为其他状态，且必须有一个不可改变的原因（reason）

* 5、new Promise(executor =(resolve, reject) => {resolve(value)}), resolve(value)将状态设置为fulfilled

* 6、new Promise(executor = (resolve, reject) => {reject(reason)}), reject(reason)将状态设置为rejected

* 7、若是executor运行异常执行reject()

* 8、thenable:then(onFulfilled, onRejected)

  + onFulfilled：status为fulfilled，执行onFullfilled，传入value

  + onRejected：status为rejected，执行onRejected，传入reason

