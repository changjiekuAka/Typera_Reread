#  message

对于普通的对象，可以直接对其进行更改

但如果一个消息类型的对象里面有着一个又是消息类型的对象的话，就不可以直接像普通的那样就行直接修改，需要使用mutable_XXX()函数，返回一个指针，通过这个指针修改成员对象的数据 



# repeated

声明这是一个列表

```protobuf
repeated user friend_list
```

往列表中添加一个元素

```protobuf
add_friend_list
```

