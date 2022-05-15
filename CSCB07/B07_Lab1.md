---
slug: "lab-1"
title: "Lab 1"
weight: 1
---

# Lab 1 Notes

> **Due 05/22/2022 23:59 EST**

{{< hint info >}}

**Handy Links**:

- [Lab Handout PDF](https://q.utoronto.ca/courses/260774/files/20903707)
- [Helpful Cheatsheet for SVN and Bash](https://cscb07-tips.github.io/)
- [Installing Eclipse (Windows/Mac/Linux)](https://q.utoronto.ca/courses/260774/files/20895335?module_item_id=3678655) (You don't need to do step 4)
- [Installing SVN](https://q.utoronto.ca/courses/260774/files/20895336?module_item_id=3678656)

{{< /hint >}}

### SVN Repository Details

```bash
svn checkout -r 1 https://helixteamhub.cloud/UTSC/projects/tut1/repositories/subversion/lab1

# Username: cscb07
# Password: Cscb07#s22
```

**Note**: If you are trying to install homebrew on mac and the install is failing, make sure to turn off any vpn services you have, and make sure you're using UofT Wifi. This probably won't be an issue for an In-person tutorial, but this was a (frustrating) problem in online tutorials for students outside of Canada. Just food for thought 🍉



### Drag and Drop Issues

If you drag and drop `Point.java` in **Step 4** and the file is copied instead of linked, or a prompt isn't shown at all, *or* nothing happens. Go to the following path (menu), and make sure the config matches the screenshot
```Java
Window > Preferences > General > Workspace > Linked Resources
```

![Linked Resources Config](https://gcdnb.pbrd.co/images/CvpipJpc9YUx.png?o=1)



### Code Snippets

> Just makes it easier to copy from here than in the pdf

**Step 8** - `PointTest.java`

```java
@Test
void test1_distance() {
	Point A = new Point(1,1);
	Point B = new Point(2,1);
	assertEquals(A.distance(B), 1);
}

@Test
void test2_distance() {
	Point A = new Point(1,1);
	Point B = new Point(3,1);
	assertEquals(A.distance(B), 2);
}

@Test
void test_isInsideCircle() {
	Point A = new Point(1,0);
	Point C = new Point(0,0);
	assertTrue(A.isInsideCircle(C,2));
}
```

**Step 12** - `Point.java`

```java
// Bug 1
return Math.sqrt(Math.pow(x-other.x, 2) + Math.pow(y-other.y, 2));

// Bug 2
return distance(center) < radius;
```



### Steps 15 and 16

#### Commit Command

```bash
svn commit -m "<STUDENT_NAME> added PointTest.java and fixed 2 bugs in Point.java"
```

- You won't lose any marks if you have a different commit message, this just keeps things consistent
- Replace `<STUDENT_NAME>` (the whole thing), with the name you have on Quercus



#### How to check if you committed successfully

```bash
svn update
svn log
# Check to see if your commit is in the history
```

You can also visit the [Repository Website](https://helixteamhub.cloud/UTSC/projects/tut1/repositories/lab1/history/HEAD) and view your specific commit to see if the files modified were correct

```yaml
Username:   cscb07
Password: 	Cscb07#s22
Company ID: UTSC
```


**Note**: FYI, almost *EVERYONE* will run into the out-of-date error in step 15. This is because everyone is committing to a single branch. So you'd often run into a case where while you're working on the lab, after checking out the repo. Another student commits before you. When you eventually commit, you'll get the error. SO DON'T SKIP THE LAST STEP!
