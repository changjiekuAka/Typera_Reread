```c
/**
 * \brief create a handle to used communicate with zookeeper.
 * 
 * This method creates a new handle and a zookeeper session that corresponds
 * to that handle. Session establishment is asynchronous, meaning that the
 * session should not be considered established until (and unless) an
 * event of state ZOO_CONNECTED_STATE is received.
 * \param host comma separated host:port pairs, each corresponding to a zk
 *   server. e.g. "127.0.0.1:3000,127.0.0.1:3001,127.0.0.1:3002"
 * \param fn the global watcher callback function. When notifications are
 *   triggered this function will be invoked.
 * \param clientid the id of a previously established session that this
 *   client will be reconnecting to. Pass 0 if not reconnecting to a previous
 *   session. Clients can access the session id of an established, valid,
 *   connection by calling \ref zoo_client_id. If the session corresponding to
 *   the specified clientid has expired, or if the clientid is invalid for 
 *   any reason, the returned zhandle_t will be invalid -- the zhandle_t 
 *   state will indicate the reason for failure (typically
 *   ZOO_EXPIRED_SESSION_STATE).
 * \param context the handback object that will be associated with this instance 
 *   of zhandle_t. Application can access it (for example, in the watcher 
 *   callback) using \ref zoo_get_context. The object is not used by zookeeper 
 *   internally and can be null.
 * \param flags reserved for future use. Should be set to zero.
 * \return a pointer to the opaque zhandle structure. If it fails to create 
 * a new zhandle the function returns NULL and the errno variable 
 * indicates the reason.
 */
ZOOAPI zhandle_t *zookeeper_init(const char *host, watcher_fn fn,
  int recv_timeout, const clientid_t *clientid, void *context, int flags);
```



发送zk的连接和zk连接响应是异步的，也就是说调用这个函数，创建句柄成功并不代表客户端就连接zk服务中心成功，只有客户端实际收到服务端的请求连接的响应时，才表明连接成功了

zookeeper_mt为zk多线程版本：包括三种线程

一、API线程也就是调用API接口的线程

二、网络IO线程，比如上述API接口，在实现网络请求时会开辟一个线程来执行对Server请求，底层调用pthread_create函数，其中这里使用poll，因为它是一个客户端程序不需要支持高并发，使用poll监听Server的请求消息

三、Watcher线程，也就是执行绑定的watcher函数的线程

```c
/**
 * \brief create a node synchronously.
 * 
 * This method will create a node in ZooKeeper. A node can only be created if
 * it does not already exists. The Create Flags affect the creation of nodes.
 * If ZOO_EPHEMERAL flag is set, the node will automatically get removed if the
 * client session goes away. If the ZOO_SEQUENCE flag is set, a unique
 * monotonically increasing sequence number is appended to the path name.
 * 
 * \param zh the zookeeper handle obtained by a call to \ref zookeeper_init
 * \param path The name of the node. Expressed as a file name with slashes 
 * separating ancestors of the node.
 * \param value The data to be stored in the node.
 * \param valuelen The number of bytes in data. To set the data to be NULL use
 * value as NULL and valuelen as -1.
 * \param acl The initial ACL of the node. The ACL must not be null or empty.
 * \param flags this parameter can be set to 0 for normal create or an OR
 *    of the Create Flags
 * \param path_buffer Buffer which will be filled with the path of the
 *    new node (this might be different than the supplied path
 *    because of the ZOO_SEQUENCE flag).  The path string will always be
 *    null-terminated. This parameter may be NULL if path_buffer_len = 0.
 * \param path_buffer_len Size of path buffer; if the path of the new
 *    node (including space for the null terminator) exceeds the buffer size,
 *    the path string will be truncated to fit.  The actual path of the
 *    new node in the server will not be affected by the truncation.
 *    The path string will always be null-terminated.
 * \return  one of the following codes are returned:
 * ZOK operation completed successfully
 * ZNONODE the parent node does not exist.
 * ZNODEEXISTS the node already exists
 * ZNOAUTH the client does not have permission.
 * ZNOCHILDRENFOREPHEMERALS cannot create children of ephemeral nodes.
 * ZBADARGUMENTS - invalid input parameters
 * ZINVALIDSTATE - zhandle state is either ZOO_SESSION_EXPIRED_STATE or ZOO_AUTH_FAILED_STATE
 * ZMARSHALLINGERROR - failed to marshall a request; possibly, out of memory
 */
ZOOAPI int zoo_create(zhandle_t *zh, const char *path, const char *value,
        int valuelen, const struct ACL_vector *acl, int flags,
        char *path_buffer, int path_buffer_len);
```

该函数的执行为同步的，其中flags的设置，表示设置为临时性节点或者是永久性节点，默认是永久性节点，设置ZOO_EPHEMERAL后为临时性节点

<img src="C:\Users\ZZZXXXJJ\AppData\Roaming\Typora\typora-user-images\image-20240416133601144.png" alt="image-20240416133601144" style="zoom:50%;" />

使用Get命令查看节点信息可以看到，节点是临时还是永久，这个是永久的