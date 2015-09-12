---
layout: post
title: "Objective-C block usage"
date: 2015-08-15 14:43:40 +0800
comments: true
categories: Objective-C, block
---
## Block syntax

> Block description

### 1. Use block as callback

```
int (^maxBlock)(int, int);
maxBlock = ^(int a, int b) {
	return a > b ? a : b;
}

NSLog(@"max is %d", maxBlock(89, 22));

```

### 2. Use block as parameter
```
- (void)someMethod:(void(^)(void))action;

[self someMethod:^() {
	NSLog(@"Do some operation in block.");
}];


```
### 3. Use block in recurcive callback

```
// It looks well.
int(^fibonacci)(int) = ^(int n) {
	if (n <= 1) return n;

	return fibonacci(n - 1) + fibonacci(n - 2);
}

fibonacci(10);

//It raise exception in runtme.
//CompositeTests testExample]_block_invoke(.block_descriptor=0x00007fe0da577710, folder=0x00007fe0da575c50) + 766 at CompositeTests.m:49, queue = 'com.apple.main-thread', stop reason = EXC_BAD_ACCESS (code=1, address=0x10)

```

> So, you should add `static` before block declaring, because at this time block is not created and it raise BAD_ACCESS to memory.



