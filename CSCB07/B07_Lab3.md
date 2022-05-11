---
slug: "lab-3"
title: "Lab 3"
weight: 1
---

# Lab 3 Notes
> Lab 3, **Due 10/8/2021 23:59 EST**
- Handy Links:

  - https://www.regextester.com/
  - https://cheatography.com/davechild/cheat-sheets/regular-expressions/    
  - https://www.baeldung.com/java-buffered-reader (if `Scanner` is not working)   

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

