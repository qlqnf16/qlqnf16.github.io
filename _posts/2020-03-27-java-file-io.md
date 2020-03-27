---
layout: post
title: "JAVA 파일 입력"
category: [java]
---

JAVA 에서 파일로 입력을 받는 여러가지 방법

---

JAVA는 알고리즘 문제를 풀기 위해 접한 게 처음이어서, `Scanner(System.in)`을 이용한 입력에만 익숙했다. 그러다 **파일입력을 받아 처리**하는 게 과제로 나왔는데, 조금 까다로웠어서 정리해놓는다.



## 한 줄씩 읽기

일반적으로 파일은 **Filereader 스트림**이 가지고 있는 `.read()`로 입력을 받는다. 하지만 `.read()` 는 글자 단위로 입력을 받는다. 그렇다면 입력을 한 줄 단위로 처리하기 위해서 어떻게 할까?

`BufferedReader` 로 버퍼를 생성하면 `.readline()` 메소드를 이용해 한 줄 단위로 접근할 수 있다.

의외로 헤맸던 점은 파일명 경로설정. `../` 같은 상대경로가 당연히 작동할 거라고 생각하고 코드를 작성했더니 파일에 접근이 제대로 안됐다(...) 그래서 `.getAbsolutePath()` 메소드로 확인해서 수정했다. 의외로 경로나 파일을 접근하는 쪽에서 에러를 발생시키지 않아서 삽질을 좀 했는데, **파일명 설정에 유의해야 한다**는 것을 알게 되었다.

```java
import java.util.Scanner;
import java.io.BufferedReader;
import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;

public class FileInput {
  		static int TC;
 
      public static void main(String[] args) {
            Scanner sc = new Scanner(System.in);
            String fileName = sc.nextLine();
            try {            	
            	File file = new File("src/algorithm/"+fileName);
            	FileReader fileReader = new FileReader(file);
            	BufferedReader bufferedReader = new BufferedReader(fileReader);
            	TC = Integer.parseInt(bufferedReader.readLine());
              for (int tcase = 0; tcase<TC; tcase++) {
              	String inputString = bufferedReader.readLine();
                solve(inputString);
              }
            } catch (FileNotFoundException e) {
						} catch (IOException e) {
								System.err.println();
						}
            sc.close();
      }
}
```



## 파일 통째로 읽기

Scanner 에 System.in 대신 **File 객체**를 넣어 파일을 통째로 읽어올 수도 있다. 이 방법을 쓰면 기존에 Scanner 로 입력받는 것과 유사하게 구현할 수 있다.

```java
import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;
 
public class FileInput {
    public static void main(String[] args){
        try{
            File file = new File("src/algorithm/test.txt");
            Scanner scan = new Scanner(file);
            while(scan.hasNextLine()){
                System.out.println(scan.nextLine());
            }
        }catch (FileNotFoundException e) {
        }
    }
}
```



### 참고 사이트

> [자바 파일 입출력 (txt파일 한 문자씩, 한 줄씩, 한 번에 읽기)](https://jeong-pro.tistory.com/69)



