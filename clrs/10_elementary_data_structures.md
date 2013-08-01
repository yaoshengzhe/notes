## Exercises
##### 10.1-2
One stack grows from left to right and the other grows from right to left, stacks are full if they meets each other.

##### 10.1-4
	ENQUEUE(Q, x)
		if Q.size > Q.length
			error "Queue is full"
		Q[Q.tail] = x
		Q.size = Q.size + 1
		if Q.tail == Q.length
			Q.tail = 1
		else
			Q.tail = Q.tail + 1
		
	DEQUEUE(Q)
		if Q.size < 1
			error "Q is empty"
		
		x = Q[Q.head]
		Q.size = Q.size - 1
		if Q.head = Q.length
			Q.head = 1
		else 
			Q.head = Q.head + 1
		return x
		
##### 10.1-5

	INSERT-FRONT(D, x)
		if D.size >= D.capacity
			error "deque is full"
		D[D.head] = x
		D.head = D.head - 1
		if D.head < 1
			D.head = D.length
		D.size = D.size + 1
		
	INSERT-END(D, x)
		if D.size >= D.capacity
			error "deque is full"
		D[D.tail] = x
		D.tail = D.tail + 1
		if D.tail > D.length
			D.tail = 1
		D.size = D.size + 1
		
	DELETE-FRONT(D)
		if D.size < 1
			error "deque is empty"
		D.size = D.size - 1
		D.head = D.head + 1
		if D.head > D.length
			D.head = 1

	DELETE-END(D)
		if D.size < 1
			error "deque is empty"
		D.size = D.size - 1
		D.tail = D.tail - 1
		if D.tail < 1
			D.tail = D.length
			
##### 10.1-6
Enqueue O(1): push to stack 1

Dequeue O(1): pop element from stack 2. if stack 2 is empty, pop all elements from stack 1 and push them into stack 2 in order and then pop stack 2.

##### 10.1-7
Push O(1): push to queue 1

Pop O(1): pop element from queue 2. if queue 2 is empty, pop all elements from queue 1 and push them into queue 2 in order and then pop queue 2.
 				
				
##### 10.2-1
INSERT: NO. DELETE: YES

##### 10.2-2

	PUSH(S, x)
		node = Node(x)
		node.next = S.head
		S.head = node
		
	POP(S)
		if S.head == Nil
			error "stack is empty"
		node = S.head
		S.head = S.head.next
		return node.val
		
##### 10.2-3
	ENQUEUE(Q, x)
		node = Node(x)
		if Q.head == Nil
			Q.head = Q.tail = node
		else
			Q.tail.next = node
			Q.tail = node
			
	DEQUEUE(Q)
		if Q.head == Nil
			error "queue is empty"
		node = Q.head
		Q.head = Q.head.next
		return node.val
		
##### 10.2-7

	REVERSE(L)
		prev = L.head
		cur = head
		while cur != Nil
			next = cur.next
			cur.next = prev
			prev = cur
			cur = next
		L.head = prev
		
##### 10.4-2

	PRINT(T)
		if T != Nil
			PRINT(T.left)
			print(T.val)
			PRINT(T.right)
			
##### 10.4-3

	PRINT(T)
		if T == Nil
			return
		Stack.add(T)
		while !Stack.isEmpty()
			cur = Stack.pop()
			print(cur.val)
			if cur.left != Nil
				Stack.push(cur.left)
			if cur.right != Nil
				Stack.push(cur.right)

##### 10.4-4
	PRINT(T)
		if T != Nil
			PRINT(T.left)
			print(T.val)
			PRINT(T.rightSib)