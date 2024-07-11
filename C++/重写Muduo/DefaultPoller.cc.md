在实现Poller的时候，其中的一个static函数的实现，结构上的设计比较好

> 注意：EventLoop     =          ChannelList          +          Poller

```c++
class Channel;

///
/// Base class for IO Multiplexing
///
/// This class doesn't own the Channel objects.
class Poller : noncopyable
{
 public:
  typedef std::vector<Channel*> ChannelList;

  Poller(EventLoop* loop);
  virtual ~Poller();

  /// Polls the I/O events.
  /// Must be called in the loop thread.
  virtual Timestamp poll(int timeoutMs, ChannelList* activeChannels) = 0;

  /// Changes the interested I/O events.
  /// Must be called in the loop thread.
  virtual void updateChannel(Channel* channel) = 0;

  /// Remove the channel, when it destructs.
  /// Must be called in the loop thread.
  virtual void removeChannel(Channel* channel) = 0;

  virtual bool hasChannel(Channel* channel) const;

  static Poller* newDefaultPoller(EventLoop* loop);

  void assertInLoopThread() const
  {
    ownerLoop_->assertInLoopThread();
  }

 protected:
  typedef std::map<int, Channel*> ChannelMap;
  ChannelMap channels_;

 private:
  EventLoop* ownerLoop_;
}
```

其中这里的newDefaultPoller，为EventLoop提供一个获取默认IO复用的接口

这里在这个函数的实现上面就没有选择在Poller.cc中实现，虽说按理一个类的声明和定义都是同名文件，不过这里考虑到这个函数的实现需要包含Poller的两个子类PollPoller和EpollPoller，所以此处如果在Poller.cc中实现，就需要包含他们的头文件，在层次结构上，属于基类包含子类的头文件，不样不是一个好得设计，所以这里就单独使用了一个DefaultPoller.cc来做这个static函数的实现