# Auto-generating a regression test 

At a prior job, I wrote a complex [dependency injection](https://github.com/muir/nject)
system in Go.

The basic idea of the system is that you give it a list of functions, and 
based solely on the types of the parameters, it calls the last function in the
list.  To do that, it looks at the inputs to the last function and figures out
which other functions it needs to call to generate those inputs.  This is done
transitively.

For example:

```go
nject.Run("example",
	func() string { return "8" },
	func(s string) (int, nject.TerminalError) {
		return strconv.Atoi(s)
	},
	func (i int) {
		fmt.Println("original number was", i)
	},
)
```

[Nject](https://github.com/muir/nject) has been used for server startup 
(library singletons and such); http endpoint handling; and to create test frameworks.
Over the years, it has accumulated a lot of features.

It supports a lot of beyond that basic idea:

- wrapper functions (middleware) that invoke the rest of the call chain;
- passing return values back up the chain;
- separating initialization from repeative invocations (to better handled endpoint handlers);
- terminating the call chain when error is returned;
- side-chains to generate extras of certain types;
- reflective functions generated based on what else is in the chain;
- automatically re-ordering the call chain; and
- avoiding or requiring certain injectors

## the problem

With all of those features, the code is more than a little bit complex.  In practice,
injection chains often include more than fifty functions.  When there a bug in the 
injection system, reproducing that bug requries using the whole chain of injectors.
That's not practical for a regression test, and it would make it nearly impossible for
someone to submit a bug report.

## the solution

Rather than asking people to manually create a regression test, the system
generates a regression test automatically.

There are two ways to get the regression test.  The first is evaluating the injection
chain returns error, there is a function that will convert the error into detailed 
error that includes the regression test.  The second way, is if you accept the 
`*Debugging` type as a parameter in the injection chain, one of the fields of 
`Debugging` is the regression test.

## recent use

I added a feature recently in response to a request (automatic injector reordering).
It turned out that the feature had a bug!  

Fortunately, the bug was easy to fix since I could reproduce it with the generated
regression test:

```go
func TestRegression(t *testing.T) {
	type i003 interface {
		x003()
	}
	type s004 int
	type i005 interface {
		x005()
	}
	type s006 int
	type s009 int
	type i011 interface {
		x011()
	}
	type i013 interface {
		x013()
	}
	type i015 interface {
		x015()
	}
	type s017 int
	type s018 int
	type s020 int
	type i021 interface {
		x021()
	}
	type s022 int
	type s019 int
	type s023 int
	type s024 int
	type s025 int
	type s026 int
	type s027 int
	type s028 int
	type s029 int
	type s030 int
	type s031 int
	type s032 int
	type s033 int
	type s034 int
	type s035 int
	type i036 interface {
		x036()
	}
	type s037 int
	type s038 int
	type s039 int
	type s040 int
	type s041 int
	type s043 int
	type s044 int
	type s045 int
	type s046 int
	type s047 int
	type s048 int
	type s049 int
	type s050 int
	type s051 int
	type s007 int
	type s008 int
	type s010 int
	type s012 int
	type s014 int
	type s016 int
	type i052 interface {
		x052()
	}
	type s054 int

	wrapTest(t, func(t *testing.T) {
		called := make(map[string]int)
		var invoker func() error
		err := Sequence("regression",
			Provide("Run()error", func() TerminalError { called["Run()error"]++; return nil }),
			Provide("TCP-0", func() i003 { called["TCP-0"]++; return nil }),
			Provide("TCP-1", func(inner func() error, _ s004) {
				called["TCP-1"]++
				inner()
			}),
			Provide("TCP-2", func() i005 { called["TCP-2"]++; return nil }),
			Provide("TCP-3", func(_ i005) i005 { called["TCP-3"]++; return nil }),
			Shun(Provide("integration-before-user-0", func() s006 { called["integration-before-user-0"]++; return 0 })),
			Shun(Provide("integration-before-user-1", func() s009 { called["integration-before-user-1"]++; return 0 })),
			Provide("base-chain-0", func(_ i003) i011 { called["base-chain-0"]++; return nil }),
			Provide("base-chain-1", func(_ i003) i013 { called["base-chain-1"]++; return nil }),
			Provide("base-chain-2", func() i015 { called["base-chain-2"]++; return nil }),
			Provide("base-chain-3", func(_ i003, _ s006, _ i011) s017 { called["base-chain-3"]++; return 0 }),
			Provide("base-chain-4", func(_ i003, _ i011, _ s018, _ i015) s020 { called["base-chain-4"]++; return 0 }),
			Provide("server-chain-0", func(_ i003, _ i011) i021 { called["server-chain-0"]++; return nil }),
			Provide("server-chain-1", func() s022 { called["server-chain-1"]++; return 0 }),
			Provide("server-chain-2", func() s019 { called["server-chain-2"]++; return 0 }),
			Shun(Provide("server-chain-3", func() s023 { called["server-chain-3"]++; return 0 })),
			Provide("server-chain-4", func(_ i003, _ s023, _ i015, _ i011) s024 { called["server-chain-4"]++; return 0 }),
			Provide("server-chain-5", func(_ i011) s025 { called["server-chain-5"]++; return 0 }),
			Shun(Provide("server-chain-6", func() s026 { called["server-chain-6"]++; return 0 })),
			Provide("server-chain-7", func(_ i003, _ i015, _ s026, _ i011) s027 { called["server-chain-7"]++; return 0 }),
			Shun(Provide("server-chain-8", func() s028 { called["server-chain-8"]++; return 0 })),
			Provide("server-chain-9", func(_ i003, _ i015, _ s028, _ i011) s029 { called["server-chain-9"]++; return 0 }),
			Shun(Provide("server-chain-10", func() s030 { called["server-chain-10"]++; return 0 })),
			Provide("server-chain-11", func(_ i011) s031 { called["server-chain-11"]++; return 0 }),
			Provide("server-chain-12", func(_ i011) s032 { called["server-chain-12"]++; return 0 }),
			Shun(Provide("server-chain-13", func() s033 { called["server-chain-13"]++; return 0 })),
			Provide("server-chain-14", func(_ s020, _ i011, _ s033) s034 { called["server-chain-14"]++; return 0 }),
			Shun(Provide("server-chain-15", func() s035 { called["server-chain-15"]++; return 0 })),
			Provide("server-chain-16", func(_ i003, _ i011, _ i021, _ s035) i036 { called["server-chain-16"]++; return nil }),
			Provide("server-chain-17", func(_ i005, _ i003, _ s037, _ s022, _ i013) s038 { called["server-chain-17"]++; return 0 }),
			Provide("server-chain-18", func(_ i003) s039 { called["server-chain-18"]++; return 0 }),
			Provide("server-chain-19", func(_ i003) s040 { called["server-chain-19"]++; return 0 }),
			Provide("server-chain-20", func(_ s017, _ i013) s041 { called["server-chain-20"]++; return 0 }),
			Provide("server-chain-21", func(_ s041, _ s020, _ s039, _ s040) s043 { called["server-chain-21"]++; return 0 }),
			Provide("server-chain-22", func(_ s020, _ s039, _ s040, _ i011, _ s017) s044 { called["server-chain-22"]++; return 0 }),
			Provide("integration-before-user-4", func(_ i011, _ s044, _ i013, _ s017, _ s038, _ s025, _ i036, _ s020, _ s024, _ s034, _ s045, _ s029, _ s030, _ s031, _ s032) s046 {
				called["integration-before-user-4"]++
				return 0
			}),
			Provide("integration-before-user-5", func(_ i011, _ s044, _ i013, _ s017, _ s038, _ s025, _ i036, _ s020, _ s024, _ s045, _ s034, _ s029, _ s030, _ s031, _ s032) s047 {
				called["integration-before-user-5"]++
				return 0
			}),
			Provide("environments-0", func(_ i003, _ i011) s048 { called["environments-0"]++; return 0 }),
			Provide("environments-1", func(_ i005, _ s017, _ s046, _ s047, _ s048, _ s006) s049 { called["environments-1"]++; return 0 }),
			Provide("environments-2", func(_ s048, _ s019, _ s020) s050 { called["environments-2"]++; return 0 }),
			Provide("environments-3", func(_ i005, _ s048, _ s049, _ s050) s051 { called["environments-3"]++; return 0 }),
			Provide("integration-before-user-7", func() s019 { called["integration-before-user-7"]++; return 0 }),
			Provide("integration-user-and-more-0", func(_ i005) s007 { called["integration-user-and-more-0"]++; return 0 }),
			Provide("public-client-0", func(_ i005, _ s007) s008 { called["public-client-0"]++; return 0 }),
			Provide("public-client-1", func(_ s008) s010 { called["public-client-1"]++; return 0 }),
			Provide("integration-user-and-more-2", func(_ i005, _ i003, _ s010, _ s009) s012 { called["integration-user-and-more-2"]++; return 0 }),
			Provide("integration-user-and-more-3", func(_ s012) s014 { called["integration-user-and-more-3"]++; return 0 }),
			Provide("integration-user-and-more-4", func(_ i005, _ i003, _ s008) s016 { called["integration-user-and-more-4"]++; return 0 }),
			Provide("cluster-0", func(_ i003) i052 { called["cluster-0"]++; return nil }),
			Provide("revs-0", func(_ i003) { called["revs-0"]++ }),
			Reorder(Provide("revs-1", func() s009 { called["revs-1"]++; return 0 })),
			Provide("revs-2", func(_ i005) (s045, error) { called["revs-2"]++; return 0, nil }),
			Required(Provide("revs-3", func(_ i005, _ i003, _ s008, _ s045, _ s012) { called["revs-3"]++ })),
			Provide("revs-4", func(_ i005, _ i003, _ s019) s054 { called["revs-4"]++; return 0 }),
			Provide("user-chain-1", func(_ i005, _ i003, _ s019, _ s054) { called["user-chain-1"]++ }),
			Provide("user-chain-2", func(_ i005, _ i003, _ s019, _ s054) { called["user-chain-2"]++ }),
			Shun(NonFinal(Provide("TCP-6", func(inner func()) error {
				called["TCP-6"]++
				inner()
				return nil
			}))),
			Provide("user-chain-3", func(_ i005, _ s054, _ s014, _ s019, _ s012) { called["user-chain-3"]++ }),
		).Bind(&invoker, nil)
		if !assert.NoError(t, err, "bind error") {
			t.Log(DetailedError(err))
		}
		// invoker()
	})
}
```
