---
slug: "lab-3"
title: "Lab 3"
weight: 3
---

# Lab 3 Notes  

> **Due 06/12/2022 23:59 EST**

{{< hint info >}}

**Handy Links**:

- [Lab Handout PDF](https://q.utoronto.ca/courses/260774/files/21063015/)
- [Regex Tester](https://www.regextester.com/)
- [Regex CheatSheet](https://cheatography.com/davechild/cheat-sheets/regular-expressions/)   
- [Using `BufferedReader`](https://www.baeldung.com/java-buffered-reader) (if `Scanner` is not working)  

{{< /hint >}}

- Valid Hashtag is:

  - Start of file: **`hashtag`** + `whitespace`  
  - End of file: `whitespace` + **`hashtag`**  
  - Everything else: `whitespace` + **`hashtag`** + `whitespace`    

- **Note**: Newlines are considered white-space (`\n`)  

- Example:

  ```txt
  Line: 
  I can't wait to fail POSt at #UofT #Boundless! There's just #Too#Many#Proofs! #Boundless\n
  
  Hashtags: 
  #UofT
  #Boundless 
  ```

- Make sure to escape `\`  characters on windows

  ```java
  String path = "C:\\Users\\Navinn\\Documents\\TA\\CSCB07\\lab3\\src\\Tweets2020";
  ```

- Also escape `\` for Java Regex

  ```java
  // Example: Match one or more digits (\d)
  Pattern p = Pattern.compile("[\\d]+");
  ```

- When you make a new Writer/Reader/Scanner instance, be sure to close it after you’re done


  ```java
  // Example
  public static void main(String[] pleaseCurveTheExam) throws IOException {
      // Create the scanner
      Scanner sc = new Scanner(System.in);
  
  	// Do some stuff here ...
      
      // Close the scanner
      sc.close();
  }
  ```

