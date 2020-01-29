# Go Talk: Behavior Driven Development with Ginkgo and Gomega

Checked: No
Due date: Jan 28, 2020
Project: https://www.notion.so/DH-Become-Principal-Software-Engineer-758f72c8bc98495697ea0e2a09a6d5dc
Status: In Progress

---

## About me

![Go%20Talk%20Behavior%20Driven%20Development%20with%20Ginkgo%20an/70925392_126225155430836_6531618081427423232_o.jpg](Go%20Talk%20Behavior%20Driven%20Development%20with%20Ginkgo%20an/70925392_126225155430836_6531618081427423232_o.jpg)

![Go%20Talk%20Behavior%20Driven%20Development%20with%20Ginkgo%20an/Untitled.png](Go%20Talk%20Behavior%20Driven%20Development%20with%20Ginkgo%20an/Untitled.png)

**Roberto Jiménez Sánchez**

- Backend Engineer in **Ordering Experience Squad 🛒 at Delivery Hero**
- Background: Cloud Foundry/Knative committer and Software Engineer at IBM Cloud
- Gopher since 2014
- **Fun fact**: secretly a bartender 🍻

[Roberto Jimenez](https://twitter.com/jszroberto)

[Roberto Jimenez - Medium](https://medium.com/@totemteleko)

---

# Introduction

## Test-Driven Development (TDD)

 Make sure that your code is fixing your problem

### Development flow

1. 📝 write test
2. 🔴 watch it fail 
3. 👩‍💻 add new code
4. 🟢 test are green 
5. 🔁 Repeat

---

## Behavior-Driven Development (BDD)

- English description of tests
- Derived directly from specifications
- Comprehensive to non-technical readers

    var _ = Describe("Set", func() {
    	Describe("Contains", func() {
    		Context("When red has been added", func() {
    			It("Should contain red", func() {
    			})
    		})
    	})
    })

even if you don't TDD, consider: 

**how will others use my code?**

**Answer**: You go read the specs and then see it works as expected.

# Ginkgo (⭐3.6k)

[Ginkgo](https://onsi.github.io/ginkgo/)

Ginkgo is a BDD(Behavior Driven Development)-style testing framework for Golang, and its preferred matcher library is Gomega.


- Help you efficiently write **descriptive** and **comprehensive** tests
- Support Test Driven Development (TDD)

### How?

By improving the development flow. 

1. 📝 write test 

    **← find the place quicker (e.g. structure, readability, etc).** 

    **← write less code by reusing**

    **← don't reinvent the wheel (e.g. table tests, etc)**

2. 🔴 watch it fail 

    **← run the test or tests you want quickly**

3. 👩‍💻 add new code

    **← you are on your own 🤣**

4. 🟢 test are green 

    ← run the test or tests you want quickly

5. 🔁 Repeat

### Alternative BDD-frameworks

- Convey: 5.3k ⭐

    [smartystreets/goconvey](https://github.com/smartystreets/goconvey)

- Godog: 896 ⭐

    [DATA-DOG/godog](https://github.com/DATA-DOG/godog)

- Goblin: 652 ⭐

    [franela/goblin](https://github.com/franela/goblin)

## Getting started

1. Install Ginkgo CLI

        go get github.com/onsi/ginkgo/ginkgo

2. Bootstrapping tests in a package

        $ cd path/to/books
        $ ls
        book.go
        $ ginkgo bootstrap
        $ ls
        book.go
        **books_test.go # Generated**

        package books_test
        
        import (
            . "github.com/onsi/ginkgo"
            . "github.com/onsi/gomega"
            "testing"
        )
        
        func TestBooks(t *testing.T) {
            RegisterFailHandler(Fail)
            RunSpecs(t, "Books Suite")
        }

3. Generate specs for your code

        
        $ ginkgo generate book
        $ ls
        book.go
        **book_test.go #Generated**
        books_test.go

        package books_test
        
        import (
            . "/path/to/books"
            . "github.com/onsi/ginkgo"
            . "github.com/onsi/gomega"
        )
        
        var _ = Describe("Book", func() {
        
        })

4. Write your first expect

        package books_test
        
        import (
            . "/path/to/books"
            . "github.com/onsi/ginkgo"
            . "github.com/onsi/gomega"
        )
        
        var _ = Describe("Book", func() {
        	It("works!", func() {
        	})
        })

5. Run the tests:

        $ ginkgo #or go test
        
        === RUN TestBootstrap
        
        Running Suite: Books Suite
        ==========================
        Random Seed: 1378936983
        
        Will run 1 of 1 specs
        
        
        Ran 0 of 0 Specs in 0.000 seconds
        SUCCESS! -- 1 Passed | 0 Failed | 0 Pending | 0 Skipped
        
        --- PASS: TestBootstrap (0.00 seconds)
        PASS
        ok      books   0.019s

If you make it fail: 

    ginkgo ./.
    Running Suite: Books Suite
    ==========================
    Random Seed: 1580299170
    Will run 1 of 1 specs
    
    • Failure [0.000 seconds]
    Book
    /Users/r.jimenez/workspace/013-ginkgo-gomega/books/book_test.go:9
      works! [It]
      /Users/r.jimenez/workspace/013-ginkgo-gomega/books/book_test.go:10
    
      Must fail!
    
      /Users/r.jimenez/workspace/013-ginkgo-gomega/books/book_test.go:11
    ------------------------------
    
    
    Summarizing 1 Failure:
    
    [Fail] Book [It] works!
    /Users/r.jimenez/workspace/013-ginkgo-gomega/books/book_test.go:11
    
    Ran 1 of 1 Specs in 0.001 seconds
    FAIL! -- 0 Passed | 1 Failed | 0 Pending | 0 Skipped
    --- FAIL: TestBooks (0.00s)
    FAIL

### Some useful commands

- Trigger test execution when changes are detected

    ginkgo watch

- Just dry-run your tests

    ginkgo --dryRun

- Make the tests fail as soon as one test fails

    ginkgo --failFast

- Run a test until if fails

    ginkgo --untilItFails

- Run specs in a randomized order

    ginkgo --randomizeAllSpecs

- Configure flake attempts

    ginkgo --flakeAttempts 2

- Set a global timeout for the whole test execution

    ginkgo -timeout=100

## Anatomy of a test
- **WHEN**: X happens
- **GIVEN**: Y is true
- **THEN**: Z must be true

### WHEN

- `Describe` : individual behaviours of the code.
- `Context`: circumstances of those behaviours

    var _ = Describe("Book", func() {
        Describe("loading from JSON", func() {
            Context("when the JSON parses succesfully", func() {
                It("should populate the fields correctly", func() {
                  
                })
    
                It("should not error", func() {
                })
            })
    
            Context("when the JSON fails to parse", func() {
                It("should return the zero-value for the book", func() {
                })
    
                It("should error", func() {
                })
            })
        })
    
        Describe("Extracting the author's last name", func() {
            It("should correctly identify and return the last name", func() {
            })
        })
    })

### GIVEN

- `BeforeSuite`, `AfterSuite` common for all tests, and executed only once (e.g.  booting a database)

        package books_test
        
        import (
            . "github.com/onsi/ginkgo"
            . "github.com/onsi/gomega"
        
            "your/db"
        
            "testing"
        )
        
        var dbRunner *db.Runner
        var dbClient *db.Client
        
        func TestBooks(t *testing.T) {
            RegisterFailHandler(Fail)
        
            RunSpecs(t, "Books Suite")
        }
        
        var _ = BeforeSuite(func() {
            dbRunner = db.NewRunner()
            _ = dbRunner.Start()
        
            dbClient = db.NewClient()
            _ = dbClient.Connect(dbRunner.Address())
        })
        
        var _ = AfterSuite(func() {
            dbClient.Cleanup()
            dbRunner.Stop()
        })

- `JustBeforeEach`, `BeforeEach` , `JustAfterEach` `AfterEach` are used for common setup.
    - closures are heavily used to share variables across tests.
    - when using nested contexts, they are executed from the outermost to innermost of each type in the following order:
        1. `BeforeSuite` (just once for all tests)
        2. All `BeforeEach` 
        3.  `JustBeforeEach` 
        4. `It` 
        5. All `JustAfterEach` 
        6. All `AfterEach` 
        7. `AfterSuite` (just once for all tests). 

        var _ = Describe("Book", func() {
            var (
                book Book
                err error
                json string
            )
        
            BeforeEach(func() {
                json = `{
                    "title":"Les Miserables",
                    "author":"Victor Hugo",
                    "pages":1488
                }`
            })
        
            JustBeforeEach(func() {
                book, err = NewBookFromJSON(json)
            })
        
            Describe("loading from JSON", func() {
                Context("when the JSON parses succesfully", func() {
                    It("should populate the fields correctly", func() {
                    })
                })
        
                Context("when the JSON fails to parse", func() {
                    BeforeEach(func() {
                        json = `{
                            "title":"Les Miserables",
                            "author":"Victor Hugo",
                            "pages":1488oops
                        }`
                    })
        
                    It("should return the zero-value for the book", func() {
                    })
                })
            })
        })

### THEN

- `It` or `Specify` for a single spec

    var _ = Describe("Book", func() {
        It("can be loaded from JSON", func() {
            book := NewBookFromJSON(`{
                "title":"Les Miserables",
                "author":"Victor Hugo",
                "pages":1488
            }`)
    
    				// Check your expectations
        })
    })

This is the actual test code once all the setup and cleanup part has been defined in the context. 

## Focused & Pending Tests

Adding the prefix `F` to any `It`, `Describe` or `Context` allows to run a particular set of tests you are interested at the moment

    var _ = Describe("Book", func() {
    		// Tests within this Describe will run
        **FDescribe**("loading from JSON", func() {
            Context("when the JSON parses succesfully", func() {
                It("should populate the fields correctly", func() {
                })
    
                It("should not error", func() {
                })
            })
    
            Context("when the JSON fails to parse", func() {
                It("should return the zero-value for the book", func() {
                })
    
                It("should error", func() {
                })
            })
        })
    		// Rest of the tests are ignored
        Describe("Extracting the author's last name", func() {
            It("should correctly identify and return the last name", func() {
            })
        })
    })

**Side note**: you can remove any focused tests automatically by running: 

    $ ginkgo unfocus

In the same way, you to mark one or multiple tests as `Pending` with the prefix `P` to ignore them:

    var _ = Describe("Book", func() {
        Describe("loading from JSON", func() {
            Context("when the JSON parses succesfully", func() {
                It("should populate the fields correctly", func() {
                })
    
                It("should not error", func() {
                })
            })
    
            Context("when the JSON fails to parse", func() {
                It("should return the zero-value for the book", func() {
                })
    
                It("should error", func() {
                })
            })
        })
    		// Ignore all the tests inside this Describe and run the rest
        **PDescribe**("Extracting the author's last name", func() {
            It("should correctly identify and return the last name", func() {
            })
        })
    })

## How to convert standard tests to Ginkgo tests?

    $ ginkgo convert path/to/mypackage

(Some things in life can be pretty increadible simple)

# Gomega (⭐1k)

Gomega is a **matcher**/**assertion** library. It is best paired with the Ginkgo BDD test framework, but can be adapted for use in other contexts too.

[Gomega](http://onsi.github.io/gomega/)

The focus of `Gomega` is on **readability** and **modularity**. 

### Alternatives

- Testify (⭐️9.5k)

[stretchr/testify](https://github.com/stretchr/testify)

## Matchers

- Matchers for anything you can expect as you would expect like `Equal` , `BeNil`, `BeEmpty` , `ContainElement`, `BeTrue`, `BeFalse` , `MatchJSON`, etc.
- Matchers can be combined as well.

    MatchError(ContainSubstring("beginning of my error"))

- You can define **custom GomegaMatchers** by implement `GomegaMatcher` from [`github.com/onsi/gomega/types`](http://github.com/onsi/gomega/types)

More here: 

[Package gomega](https://godoc.org/github.com/onsi/gomega)

## Synchronous assertions

Assertions start with `Expect` and follow the following syntax:

    **Expect**(foo).**To**(Equal("foo"))
    
    // For the opposite
    
    **Expect**(foo).**ToNot**(Equal("bar"))

### Common assertions

- Check errors

    err := DoSomething()
    Expect(err).ToNot(HaveOcurred())
    
    // or alternatively
    
    Expect(DoSomething()).To(Succeed())
    
    // Or check a concrete error
    
    Expect(err).To(MatchError("expected error"))
    
    // Check if a concrete error contains some substring
    err := errors.New("didn't work: because you weren't lucky")
    
    Expect(err).To(MatchError(ContainSubstring("didn't work")))

Notice that we don't have to pass the value to the matchers. 

- Checking maps

    Expect(m).To(HaveKey("foo"))
    Expect(m).To(HaveKeyWithValue("foo", "bar"))
    
    
    // Checking multiple things at once
    Expect(m).To(
    	SatisfyAll(
    		HaveKey("foo"),
    		HaveKey("bar"),
    	),
    )

## Asynchronous assertions

### `Eventually`

Checks if the assertion eventually passes.

    Eventually(func() []int {
        return thing.SliceImMonitoring
    }).Should(HaveLen(2))
    
    Eventually(func() string {
        return thing.Status
    }).ShouldNot(Equal("Stuck Waiting"))

### `Consistently`

checks that an assertion passes for a period of time

    Consistently(func() []int {
        return thing.MemoryUsage()
    }).Should(BeNumerically("<", 10))

## Timeout and Polling interval

You can configure a polling internal and a timeout in both `Eventually` and `Consistently`

    **DURATION** := time.Second()
    **POLLING_INTERVAL** := 100 * time.Millisecond()
    
    Consistently(func() []int {
        return thing.MemoryUsage()
    }, **DURATION**, **POLLING_INTERVAL**).Should(BeNumerically("<", 10))

## Gexec

Testing external processes.

[Gomega](https://onsi.github.io/gomega/#gexec-testing-external-processes)

## Ghttp

Testing HTTP Clients

[Gomega](https://onsi.github.io/gomega/#ghttp-testing-http-clients)

## Gbytes

Testing streaming buffers

[Gomega](https://onsi.github.io/gomega/#gbytes-testing-streaming-buffers)

# Q&A

# Additional Material

[https://www.youtube.com/watch?v=R-j1phppdzI](https://www.youtube.com/watch?v=R-j1phppdzI)

[https://www.youtube.com/watch?v=yBPgv-1htuU](https://www.youtube.com/watch?v=yBPgv-1htuU)

[testingallthethings/013-ginkgo-gomega](https://github.com/testingallthethings/013-ginkgo-gomega)

[Session 2 - TDD, Mocking & Dependency Injection](https://www.youtube.com/watch?v=uFXfTXSSt4I)
