# Lab Report 4

My markdown-parser repo: https://github.com/rmccrystal/markdown-parser
Reviewed markdown-parser repo: https://github.com/ujik500/markdown-parser

## Snippet 1

According to commonmark, snippet 1 should contain the following links: ``"\`google.com", "google.com", "ucsd.edu"``

The code for the test is as follows:

```java
    @Test
    public void testSnippet1() {
        assertMarkdown(Path.of("snippet1.md"), List.of("`google.com", "google.com", "ucsd.edu"));
    }
```

The assertMarkdown function for all tests is the following:

```java
    void assertMarkdown(Path path, List<String> expected) {
        final String content;
        try {
            content = Files.readString(path);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
        final var links = MarkdownParse.getLinks(content);
        assertArrayEquals(expected.toArray(), links.toArray());
    }
```


### My implementation

The test fails on the assertArrayEquals function inside assertMarkdown

```
arrays first differed at element [0];
Expected :`google.com
Actual   :url.com
```

### Other implementation

The test fails on the assertArrayEquals function inside assertMarkdown

```
arrays first differed at element [0]; 
Expected :`google.com
Actual   :url.com
```

## Snippet 2

According to commonmark, snippet 2 should contain the following links: ``"a.com", "a.com(())", "example.com"``

The code for the test is as follows:

```java
    @Test
    public void testSnippet2() {
        assertMarkdown(Path.of("snippet2.md"), List.of("a.com", "a.com(())", "example.com"));
    }
```

### My implementation

The test fails on the assertArrayEquals function inside assertMarkdown

```
array lengths differed, expected.length=3 actual.length=2; arrays first differed at element [1]; 
Expected :a.com(())
Actual   :a.com((
```

### Other implementation

The test fails on the assertArrayEquals function inside assertMarkdown

```
array lengths differed, expected.length=3 actual.length=2; arrays first differed at element [1]; 
Expected :a.com(())
Actual   :a.com((
```

## Snippet 3

According to commonmark, snippet 3 should contain the following link: ``"https://sites.google.com/eng.ucsd.edu/cse-15l-spring-2022/schedule"``

The code for the test is as follows:

```java
    @Test
    public void testSnippet3() {
        assertMarkdown(Path.of("snippet3.md"), List.of("https://sites.google.com/eng.ucsd.edu/cse-15l-spring-2022/schedule"));
    }
```

### My implementation

The test fails on the assertArrayEquals function inside assertMarkdown

```
array lengths differed, expected.length=1 actual.length=0; arrays first differed at element [0]; 
Expected :https://sites.google.com/eng.ucsd.edu/cse-15l-spring-2022/schedule
Actual   :end of array
```

### Other implementation

The test fails on the assertArrayEquals function inside assertMarkdown

```
array lengths differed, expected.length=1 actual.length=3; arrays first differed at element [0]; 
```

# Questions
1. Do you think there is a small (<10 lines) code change that will make your program work for snippet 1 and all related cases that use inline code with backticks? If yes, describe the code change. If not, describe why it would be a more involved change.

Yes, keep track of the indices of all backticks. If a token is inside two backticks disregard it and keep parsing after the backtick.

2. Do you think there is a small (<10 lines) code change that will make your program work for snippet 2 and all related cases that nest parentheses, brackets, and escaped brackets? If yes, describe the code change. If not, describe why it would be a more involved change.

Yes, once an open parenthesis is found, keep track of the indices of all open and close parentheses. If a token is inside two parentheses disregard it and keep parsing after the close parenthesis.

4. Do you think there is a small (<10 lines) code change that will make your program work for snippet 3 and all related cases that have newlines in brackets and parentheses? If yes, describe the code change. If not, describe why it would be a more involved change.

Yes. Keep track of one line spaces between the open and close brackets. If this is the case disregard the newline and keep parsing.