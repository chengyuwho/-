# 暑修資料結構 11124228
# 期中報告第六題
Python實現雙向鏈表(圖解)
# 1:初始化鏈表
定義節點結構:指針域pre、next和數據域data

為方便操作添加了head和tail節點,初始化時head.next->tail,tail.pre->next

      def __init__(self):
        head = Node()
        tail = Node()
        self.head = head
        self.tail = tail
        self.head.next = self.tail
        self.tail.pre = self.head

![01](https://github.com/chengyuwho/-/blob/64709487cab7dd146b8a9ef5a2392b5287ec0f97/1.png)
# 2:獲取鏈表長度
起始head，每有一個節點，length＋1

    def append(self, data):
        node = Node(data)
        pre = self.tail.pre
        pre.next = node
        node.pre = pre
        self.tail.pre = node
        node.next = self.tail
        return node
# 3:追加節點
因為有tail 節點，所以找到tail.pre 節點就好了

    def insert(self, index, data):
        length = len(self)
        if abs(index + 1) > length:
            return False
        index = index if index >= 0 else index + 1 + length

        next_node = self.get(index)
        if next_node:
            node = Node(data)
            pre_node = next_node.pre
            pre_node.next = node
            node.pre = pre_node
            node.next = next_node
            next_node.pre = node
            return node

![01](https://github.com/chengyuwho/-/blob/fd02646ea99a7e0c003aeea14c1afeda9acb3f33/2.png)
# 4:獲取節點
獲取節點要判斷index正負值

    def delete(self, index):
        node = self.get(index)
        if node:
            node.pre.next = node.next
            node.next.pre = node.pre
            return True

        return False
# 5:設定節點
找到當前節點賦值即可

    def __reversed__(self):
        pre_head = self.head
        tail = self.tail

        def reverse(pre_node, node):
            if node:
                next_node = node.next
                node.next = pre_node
                pre_node.pre = node
                if pre_node is self.head:
                    pre_node.next = None
                if node is self.tail:
                    node.pre = None
                return reverse(node, next_node)
            else:
                self.head = tail
                self.tail = pre_head

        return reverse(self.head, self.head.next)
# 6:插入節點
插入節點需要找到插入節點的前一個節點pre_node和後一個節點next_node（索引index的正負，前一節點不同，需要判斷一下），

然後將pre_node.next–>node,node.pre->pre_node;next_node.pre–>node,node.next–>next_node

    def __len__(self):
        length = 0
        node = self.head
        while node.next != self.tail:
            length += 1
            node = node.next
        return length

![01](https://github.com/chengyuwho/-/blob/6cb33c008e07bda3191986b6090ba6eee6482b2b/3.png)
# 7:刪除節點
刪除節點，也要區分一下索引的正負。找到當前節點的前一個節點pre_node和後一個節點next_node，將pre_node.nex–>next_node即可

    def get(self, index):
        length = len(self)
        index = index if index >= 0 else length + index
        if index >= length or index < 0: return None
        node = self.head.next
        while index:
            node = node.next
            index -= 1
        return node

![01](https://github.com/chengyuwho/-/blob/1f5f70e4c23839b39df82351d7da6326272285ff/4.png)
# 8:反轉鏈表
反轉鏈表的實現有多種方式，比較簡單的就是生成一個新的鏈表－－》可以用數組存儲所有節點讓後倒序生成新的鏈表

在這里用下面這種方式生產：

可能有點繞

1.node.next –> node.pre；node.pre –> node.next（遞歸）

2.head.next –> None；tail.pre –> None

3.head–>tail；tail–>head

    def set(self, index, data):
        node = self.get(index)
        if node:
            node.data = data
        return node

![01](https://github.com/chengyuwho/-/blob/54e5ed3a53e7ea8e0f3f6cb17d9f8327c43e5e21/5.png)
# 9:清空鏈表
類似初始化

    def clear(self):
        self.head.next = self.tail
        self.tail.pre = self.head

![01](https://github.com/chengyuwho/-/blob/9c50cc3028589dba47e7360ebc3ffc20bc5cc35f/6.png)
# 結果
![01](https://github.com/chengyuwho/-/blob/63e16f946ad65aa18ff06781b234fb73f07a1524/%E7%B5%90%E6%9E%9C.png)
