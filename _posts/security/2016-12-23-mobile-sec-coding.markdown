---
layout: post
title: 모바일앱 보안코딩
category: security
comments: true
description:   보안 코딩
tags:
    - 취약점
    - andorid
    - 시큐어 코딩
---



### andorid-java 시큐어 코딩 가이드
<br><br>

| 뷴류 | 취약점 명칭 | 위험도 |CWE-ID|
| ---- | ---- | ---- |---- |
|입력데이터 검증 및 표현|상대 디렉터리 경로 조작| 매우 높음|CWE-23 |
|입력데이터 검증 및 표현|절대 디렉터리 경로 조작| 매우 높음|CWE-36|
|API악용|Null 매개변수 미검사| 높음|CWE-398|
|API악용|equals()와 hasCode()하나만 정의|높음|CWE-581|
|보안특성|기밀 정보의 단순한 텍스트전송 |높음|CWE-319|
|보안특성|취악한 암호화 알고리즘의 사용  |높음|CWE-327|
|보안특성|적절하지 않은 난수 값의 사용  |높음|CWE-330|
|보안특성|전역적으로 접근가능한 파일  |높음| -|
|보안특성|외부에서 접근하여 활성화 가능한 컴포넌트 |높음|-|
|보안특성|공유 아이디에 의한 접근제어통과 |높음|-|
|시간 및 상태|경쟁 조건 : 검사시점과 사용시점 |높음|CWE-367|
|시간 및 상태 | 제대로 제어 되지 않은 재귀 |높음|CWE-674|
|에러처리 |오류 메시지를 통한 정보 노출 |높음|CWE-209|
|에러처리 |오류 상황에 대한 처리  부재 |높음|CWE-390|
|코드 품질 |널 포인터 역 참조 |높음|CWE-476|
|캡슐화 |공용 메소드로부터 리턴 된 private 배열 –유형필드 |높음|CWE-495|
|캡슐화 |private 배열-유형 필드에 공용 데이터 할당 |높음|CWE-496|
|캡슐화 |시스템 데이터 정보누출 |높음|CWE-497|


#### 1.입력데이터 검증및 표현 상대 디텍토리 조작
  - 안전하지 않음 코드
    - 문제점 : 외부의 입력을 통하여 “디렉터리 경로 문자열” 생성이 필요한 경우,
외부 입력 에서 경로조작에 사용될 수 있는 문자를 필터링하지 않으면, 경로에 
대한 문자열이 입력되어 시스템 정보누출, 서비스 장애 등을 유발 시킬 수 있다.
  - 샘플코드 

    ```java
      public void f(Properties request) {
        String name = request.getProperty("filename");
        if( name != null ) {
          File file = new File("/usr/local/tmp/" + name);
          file.delete();
        }
      }
    ```
  - 안전한 코드 
    - 해결코드 : 외부 입력값에 대하여 Null 여부를 체크하고, 외부에서 입력되는 파일이름(name)에서 상대경로(/,\\,&등의 특수문자)를 설정 할 수 없도록 replaceAll을 이용하여 특수문자를 제거한다.
  - 샘플코드 

    ```java
        public void f(Properties request) {
            String name = request.getProperty("filename");
            String dentry = "/usr/local/tmp";
            if ( name != null && !"".equals(name) ) {
                name = name.replaceAll("/", "");
                name = name.replaceAll("\\", "");
                name = name.replaceAll(".", " ");
                name = name.replaceAll("&", " ");
                name = name + "-report";
                File file = new File(dentry + name);
                if (file != null) file.delete();
            }
        }
    ```


<br>

#### 2.입력데이터 검증및 표현 절대 디텍토리 조작
  - 안전하지 않음 코드
    - 문제점 : 외부의 입력으로부터 직접 파일을 생성하게 되는 경우 임의의 파일 이름을 입력 받을 수 있도록  되어있어, 다른 파일에 접근이 가능 해져 의도하지 않은 정보가 노출될 수 있다.
  - 샘플코드 

    ```java
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        File file = new File(android.os.Environment.getExternalStorageDirectory(), "inputFile");
        try {
            InputStream is = new FileInputStream(file);
            Properties props = new Properties();
            props.load(is);
            String name = props.getProperty("filename");
            file = new File("/usr/local/tmp/" + name);
            file.delete();
            is.close();
        } catch (IOException e) {
            Log.w("Error", "", e);
        }
    }
    ```

  - 안전한 코드 
    - 해결코드 : 외부의 입력이 파일이름으로 사용될 경우 절대 경로명이 사용되지 못하도록, 문자열이 “\”또는 “/”를 포함하거나 해당 문자열로 시작할 경우 관련동작 수행을 거부하는 것이 바람직하다.

  - 샘플코드 

    ```java
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            File file = new File(android.os.Environment.getExternalStorageDirectory(), "inputFile");
            try {
                InputStream is = new FileInputStream(file);
                Properties props = new Properties();
                props.load(is);
                String name = props.getProperty("filename");
                if (name.indexOf("/") <0) {
                    file = new File(name);
                    file.delete();
                    is.close();
            } catch (IOException e) {
                Log.w("Error", "", e);
            }
        }
    ```

#### 3. API 악용 - null 매개변수 미검사
  - 안전하지 않음 코드
    - 문제점 : Object.equals(), Comparable.compareTo(), omparator.compare()에서는 매개변수를 null과 비교 해야한다.
  - 샘플코드 

    ```java
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
        }
        public boolean equals(Object object) {
            return (toString().equals(object.toString()));
        }
    ```

  - 안전한 코드 
    - 해결코드 : 매개변수를 null과 비교하는 코드를 작성한다.
  - 샘플코드 

    ```java
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
        }
        public boolean equals(Object object){
            if(object != null)
                return (toString().equals(object.toString()));
            else return false ;
        }
    ```

#### 4. API 악용 - equals와 hasCode정의 
  - 안전하지 않음 코드
    - 문제점 : equals와 hasCode를 같이 사용하지 않았다[참고링크](http://egloos.zum.com/playpc/v/1173713)
  - 샘플코드 

    ```java
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
        }
        public boolean equals(Object obj) {
            if (obj == null)
                return false;
            int i1 = this.hashCode();
            int i2 = obj.hashCode();
            if (i1 == i2)
                return true;
            else
                return false;
        }
    ```

  - 안전한 코드 
    - 해결코드 : equals와 hasCode를 같이 작성하여해결한다.
  - 샘플코드 

    ```java
        public boolean equals(Object obj) {
            if (obj == null)
                return false;
            int i1 = this.hashCode();
            int i2 = obj.hashCode();
            if (i1 == i2)
                return true;
            else
                return false;
        }
        @Overide
        public int hashCode() {
            return new HashCodeBuilder(17, 37).toHashCode();
        }
    ```

#### 5. 보안특성 - 기밀정보의 단순 텍스트 전송 
  - 안전하지 않음 코드
    - 문제점 : 443 Port로 데이터 외부 전송코드 스니핑이 발생할 수 있다
  - 샘플코드 

    ```java
        public void onCreate(Bundle savedInstanceState) {
            int port = 443;
            String hostname = "hostname";
            Socket socket = new Socket(hostname, port);

            InputStream in = socket.getInputStream();
            OutputStream out = socket.getOutputStream();
            // Read from in and write to out...
            in.close();
            out.close();
        }
    ```

  - 안전한 코드 
    - 해결코드 : 민감한 정보를 전달 할 때에는  일반 소켓보다는 SSL을 사용하여 전송한다.    
  - 샘플코드 

    ```java
    public void onCreate(Bundle savedInstanceState) {
        int port = 443;
        String hostname = "hostname";
        SocketFactory socketFactory = SSLSocketFactory.getDefault();
        Socket socket = socketFactory.createSocket(hostname, , port);
        InputStream in = socket.getInputStream();
        OutputStream out = socket.getOutputStream();
        // Read from in and write to out...
        in.close();
        out.close();
    }
    ```

#### 6. 보안특성 - 취약한 암호화 알고리즘의 사용
  - 안전하지 않음 코드
    - 문제점 : 보안적으로 취약하거나 위험한 암호화 알고리즘을 사용했다. RC2,RC4,RC6, MD4,MD5,SHA1,DES.
  - 샘플코드 
  
    ```java
        public byte[] encrypt(byte[] msg, Key k) {
            byte[] rslt = null;

            try {
                //DES 등의 낮은 보안 수준의 알고리즘을 사용하는 것은 안전하지 않다
                Cipher c = Cipher.getInstance("DES");
                c.init(Cipher.ENCRYPT_MODE, k);
                rslt = c.update(msg);
            } catch (InvalidKeyException e) {
            }
            return rslt;
        }
    ```

  - 안전한 코드 
    - 해결코드 : 취약하다고 알려진 알고리즘 대신 AES 알고리즘을 최소한 128비트 길이를 이용하여 사용하는 것이 바람직하다
  - 샘플코드 
    ```java
    public byte[] encrypt(byte[] msg, Key k) {
         byte[] rslt = null;

        try {
            //낮은 보안 수준의 DES알고리즘을 높은 보안 수준의 AES
            // 알고리즘으로 대체한다
            Cipher c = Cipher.getInstance("AES/CBC/PKCS5Padding");
            c.init(Cipher.ENCRYPT_MODE, k);
            rslt = c.update(msg);
        } catch (InvalidKeyException e) {
        }
        return rslt;
    }
    ```    

#### 7. 보안특성 - 적절하지 않은 난수값의 사용
  - 안전하지 않음 코드
    - 문제점 : seed를 설정 할 수 없기 때문에 위험.
  - 샘플코드 
    ```java
    public double roledice() {
         return Math.random();
    }
    ```

  - 안전한 코드 
    - 해결코드 : seed를 설정 하는 코드를 사용.
  - 샘플코드 
    ```java
    public int roledice() {
         Random r = new Random();
        // setSeed
        r.setSeed(new Date().getTime());
        return (r.nextInt()%6) + 1;
    }
    ```        

#### 8. 보안특성 - 전역적으로 접근 가능한 파일
  - 안전하지 않음 코드
    - 문제점 : MODE_WORLD_READABLE은 다른 응용프로그램이 접근가능
  - 샘플코드 
    ```java
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        try {
            FileOutputStream fOut = openFileOutput("test",  MODE_WORLD_READABLE);
            OutputStreamWriter out1 = new OutputStreamWriter(fOut);
            out1.write("Hello World");
            out1.close();
            fOut.close();
        } catch (Throwable t) {
        }
    }
    ```
  - 안전한 코드 
    - 해결코드 : MODE_PRIVATE을 사용하면 외부에서 접근이 불가능하다


  - 샘플코드 
    ```java
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            try {
                FileOutputStream fOut = openFileOutput("test", MODE_PRIVATE);
                OutputStreamWriter out1 = new OutputStreamWriter(fOut);
                out1.write("Hello World");
                out1.close();
                fOut.close();
            } catch (Throwable t) {
            }
        }
    ```        

#### 9. 보안특성 -외부에서 접근하여 활성화 가능한 컴포넌트
  - 안전하지 않음 코드
    - 문제점 : 
  - 샘플코드 
    ```java
        <service android:name=".syncadapter.SyncService" android:exported="true">
    ```
  - 안전한 코드 
    - 해결코드 : 

  - 샘플코드 
    ```java
     <service android:name=".syncadapter.SyncService" android:exported="false">
    ```        

#### 10. 보안특성 - 공유 아이디에 의한 접근제어 통과
  - 안전하지 않음 코드
    - 문제점 : 
  - 샘플코드 
    ```java
    <manifest 
        xmlns:android=http://schemas.android.com/apk/res/android
        package="com.example.android.apis"
        android:versionCode="1"
        android:versionName="1.0"
        android:sharedUserId="android.uid.developer1">
    ```

  - 안전한 코드 
    - 해결코드 : 

  - 샘플코드 
    ```java
    <manifest xmlns:android=http://schemas.android.com/apk/res/android
        package="com.example.android.apis"
        android:versionCode="1"
        android:versionName="1.0">
    <!-- android:sharedUserId="android.uid.developer1“ -->
    ```        

#### 11. 보안특성 - 경쟁조건: 검사시점과 사용시점
  - 안전하지 않음 코드
    - 문제점 : 
  - 샘플코드 
    ```java
    public class UA367 extends Activity {
        @override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            FileAccessThread fileAccessThread = new FileAccessThread();
            FileDeleteThread fileDeleteThread = new FileDeleteThread();
            fileAccessThread.start();
            fileDeleteThread.start();
        }
    }
    class FileAccessThread extends Thread {
        public void run() {
            try {
                File f = new File("Test_367.txt");
                if (f.exists()) { 
                    BufferedReader br = new BufferedReader(new FileReader(f));
                    br.close();
                }
            } catch(FileNotFoundException e) {
                System.out.println("Exception Occurred");
            }
        }
    }

    class FileDeleteThread extends Thread {
        public void run() {
            try {
                File f = new File("Test_367.txt");
                if (f.exists()) { 
                    f.delete();
                }
            } catch(FileNotFoundException e) {
                System.out.println("Exception Occurred");
            } catch(IOException e) {
                System.out.println("Exception Occurred");
            }
        }
    }
    ```

  - 안전한 코드 
    - 해결코드 : 

  - 샘플코드 
    ```java
    public class SA367 extends Activity {
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
                FileAccessThread fileAccess = new FileAccessThread();
                Thread first = new Thread(fileAccess);
                Thread second = new Thread(fileAccess);
                Thread third = new Thread(fileAccess);
                Thread fourth = new Thread(fileAccess);
                first.start();
                second.start();
                third.start();
                fourth.start();
        }  
    }

    class FileAccessThread implements Runnable {
        public synchronized void run() {
            try {
                File f = new File("Test.txt");
                if (f.exists()){
                    Thread.sleep(100);
                    BufferedReader br = new BufferedReader(new FileReader(f));
                    System.out.println(br.readLine());
                    br.close();
                    f.delete();
                }
            } catch (IOException e) {
                System.err.println("IOException occured");
            }
        }
    }
    ```       

#### 12. 시간 및 상태 - 제대로 제어되지 않은 재귀
  - 안전하지 않음 코드
    - 문제점 : 
  - 샘플코드 
    ```java
    public int factorial(int n) {
        return n * factorial(n - 1);
    }
    ```

  - 안전한 코드 
    - 해결코드 : 

  - 샘플코드 
    ```java
    public int factorial(int n) {
        int i;
        if (n == 1) {
	        i = 1;
        } else {
	        i = n * factorial(n - 1);
        }
        return i;
    }
    ```       

#### 13. 에러처리 - 오류 메세지를 통한 정보 노출 
  - 안전하지 않음 코드
    - 문제점 : 
  - 샘플코드 
    ```java
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        try{ 
            throw new IOException(); 
        }catch (IOException e) { 
            e.printStackTrace(); 
        }
    }
    ```

  - 안전한 코드 
    - 해결코드 : 

  - 샘플코드 
    ```java
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        try{
            throw new IOException();
        }catch (IOException e) { 
            System.out.println(“예외발생"); 
        }
    }
    ```           

#### 14. 에러처리 - 오류 상황에 대한 처리 부재
  - 안전하지 않음 코드
    - 문제점 : 
  - 샘플코드 
    ```java
    private Connection conn;
    public Connection DBConnect(String url, String id, String password) {
        try {
            String CONNECT_STRING = url + ":" + id + ":" + password;
            InitialContext ctx = new InitialContext();
            DataSource datasource = (DataSource) ctx.lookup(CONNECT_STRING);
            conn = datasource.getConnection();
        } catch (SQLException e) {
            // catch 블록이 비어있음
        } catch (NamingException e) {
            // catch 블록이 비어있음
        }
        return conn;
    }
    ```

  - 안전한 코드 
    - 해결코드 : 

  - 샘플코드 
    ```java
    private Connection conn;
    public Connection DBConnect(String url, String id, String password) {
        try {
            String CONNECT_STRING = url + ":" + id + ":" + password;
            InitialContext ctx = new InitialContext();
            DataSource datasource = (DataSource) ctx.lookup(CONNECT_STRING);
            conn = datasource.getConnection();
        } catch (SQLException e) {
            // Exception catch 이후Exception에 대한 적절한 처리가 필요
            if ( conn != null ) {
                try {
                    conn.close();              
                }catch (SQLException e1) { 
                    conn = null;
                }
            }
        } catch (NamingException e) {
        // Exception catch 이후Exception에 대한 적절한 처리가 필요
        if ( conn != null ) {
            try {
                conn.close();
            } catch (SQLException e1) { 
                conn = null; 
            }
        }
    }
    ```           

#### 15. 코드품질 - 널포인터 역참조
  - 안전하지 않음 코드
    - 문제점 : 
  - 샘플코드 
    ```java
    public void f(boolean b) {
        String cmd = System.getProperty("cmd");
        cmd = cmd.trim();
        System.out.println(cmd);
    }

    ```

  - 안전한 코드 
    - 해결코드 : 

  - 샘플코드 
    ```java
    public void f(boolean b) {
        String cmd = System.getProperty("cmd");
        if (cmd != null) { 
            cmd = cmd.trim();
            System.out.println(cmd);
        } else{
            System.out.println("null command");
        }
    }
    ```           

#### 16. 캡슐화 - 공용 메소드로 부터 리턴된 private 배열 
  - 안전하지 않음 코드
    - 문제점 : 
  - 샘플코드 
    ```java
    // private인 배열을 public인 메소드가 return한다
    private String[] colors;
    public String[] getColors() { 
	    return colors; 
    }
    ```

  - 안전한 코드 
    - 해결코드 : 

  - 샘플코드 
    ```java
    private String[] colors;
    //메소드를 private으로 하거나, 복제본 반환,수정하는 public메소드를 별도로 만든다
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        String[] newColors = getColors();
    }
    public String[] getColors() {
        String[] ret = null;
        if ( this.colors != null ) {
            ret = new String[colors.length];
            for (int i = 0; i < colors.length; i++) { ret[i] = this.colors[i]; }
        }
        return ret;
    }

    ```  

#### 17. 캡슐화 – private배열-유형필드에 공용데이터할당
  - 안전하지 않음 코드
    - 문제점 : 
  - 샘플코드 
    ```java
    // userRoles 필드는 private 이지만 public인 setUserRoles()를 통해 외부의 배열이 할당 되면 사실상 public필드가된다.
    private String[] userRoles;
    public void setUserRoles(String[] userRoles) {
        this.userRoles = userRoles;
    }
    ```

  - 안전한 코드 
    - 해결코드 : 

  - 샘플코드 
    ```java
    // 객체가 클래스의 private member를 수정하지 않도록 한다.
        private String[] userRoles;

        public void setUserRoles(String[] userRoles) {
            this.userRoles = new String[userRoles.length];
            for (int i = 0; i < userRoles.length; ++i){
                this.userRoles[i] = userRoles[i];
            }
        }
    ```  
#### 18. 캡슐화 - 시스템 데이터 정보 누출
  - 안전하지 않음 코드
    - 문제점 : 
  - 샘플코드 
    ```java
    public void f() {
        try { 
            g();
        }catch (IOException e) {
            //예외 발생시 printf(e.getMessage())를 통해 
            //오류 메시지 정보가 유출된다.
            System.err.printf(e.getMessage());
        }
    }
    private void g() throws IOException { …… }
    ```

  - 안전한 코드 
    - 해결코드 : 
  - 샘플코드 
    ```java
        public void f() {
            try { 
                g();
            }catch (IOException e) {
                // end user가 볼수 있는 오류 메시지 정보를 생성하지 않아야 한다.
                System.err.println("IOException Occured");
            }
        }
        private void g() throws IOException { …… }
    ```  
