# usva-go

`usva-go`-repository is a wrapper for [Usva's API](https://github.com/usvacloud/usva).

## Supported features

-   [ ] #### API endpoint support

    -   [ ] ##### `/` - General

        -   [ ] `/restrictions` - Get different API restrictions

    -   [ ] ##### `/file` - Files

        -   [ ] `GET /info?filename=<uuid>` - Get file's information
        -   [ ] `GET /?filename=<uuid>` - Download a file
        -   [ ] `POST /upload` - Upload a file

    -   [ ] ##### `/account` - Accounts

        -   [ ] `GET /profile` - Get current account
        -   [ ] `POST /create` - Create a new account

    -   [ ] ##### `/feedback` - Probably not going to be supported due to irrelevancy

-   [ ] #### Wrapper features

    -   [ ] Session persisting support
    -   [ ] Controlled execution with `context` package

## Usage (current concept)

### Session engine

```go
// ...

// returns usvago.SessionEngine
sessionEngine, err := usvago.NewSessionEngine(serviceEndpoint, userDataDirectory)
if err != nil {
    log.Fatal(err)
}

ctx := context.WithValue(context.Background(), "session", sessionEngine)
usvago.SomeAPIFunc(ctx, somevalue)
```



### Endpoints

- #### Common endpoints

  **Structures used in this section:**

  ```go
  type Restrictions struct {
      FilePersistDuration 	time.Duration	`json:"filePersistDuration"`
      MaxDailyUploadSize 		int32 			`json:"MaxDailyUploadSize"`
      MaxEncryptedFileSize 	int32			`json:"maxEncryptedFileSize"`
      MaxSingleUploadSize 	int32 			`json:"maxSingleUploadSize"`
  }
  ```

  -   `GET /restrictions`

      ```go
      func GetRestrictions(context.Context) (Restrictions, error)
      ```

- #### File endpoints

  ##### Structures used in this section:

  ```go
  type FileInfo struct {
      Encrypted 	bool	 		`json:"encrypted"`
      Filename 	string 			`json:"filename"`
      Locked 		bool 			`json:"locked"`
      Size		int32 			`json:"size"`
      Title 		sql.NullString 	`json:"title"`
      UploadDate  time.Time 		`json:"uploadDate"`
      ViewCount 	int32 			`json:"viewCount"`
  }
  
  type File struct {
      Body io.Reader
  }
  
  type FileUploadProperties struct {
      Title 					string
      EnableServerEncryption 	bool
      File // inherits properties from File
  }
  ```

  

  ##### Methods:

  - `GET /file/info?filename=<generated_filename>`

      ```go
      func GetFileInfo(context.Context, string) (FileInfo, error)
      ```

  - `GET /file/?filename=<generated_filename>`

      ```go
      func GetFile(context.Context, string) (File, error)
      ```

  - `POST /file/upload`

      ```go
      // UploadFile uploads file with given FileUploadProperties.
      // The name of uploaded file is returned per API response.
      func UploadFile(context.Context, FileUploadProperties) (string, error)
      ```



- #### Account endpoints

  ##### Structures/types used in this section:

  ```go
  type AccountInformation struct{
      AccountID 		string 		`json:"account_id"`
      Username 		string 		`json:"username"`
      RegisterDate 	time.Time 	`json:register_date"`
      LastLogin 		time.Time 	`json:"last_login"`
      ActivityPoints 	int 		`json:"activity_points"`
  }
  
  type Profile struct {
      Token 	string
      Account AccountInformation
  }
  
  type LoginProperties struct {
      Username string
      Password string
  }
  
  type RegisterProperties struct {
      Username string
      Password string
  }
  
  type Session struct {
      SessionID string	`json:"session_id"`
      StartDate time.Time `json:"start_date"`
  }
  
  type OwnedFileInfo File
  ```

  - `GET /account`

    ```go
    func GetProfile(context.Context) (Profile, error)
    ```

  - `POST /login`

    ```go
    func Login(context.Context, LoginProperties) (string, error)
    ```

  - `POST /register`

    ```go
    func Register(context.Context, RegisterProperties) (string, error)
    ```

  - `GET /account/files`

    ```go
    func GetFiles(context.Context) ([]OwnedFileInfo, error)
    ```

  - `GET /account/files/all`

    ```go
    func GetAllFiles(context.Context) ([]string, error)
    ```

  - `DELETE /account/sessions`

    ```go
    func DeleteSession(context.Context, string) error
    ```

  - `DELETE /account/sessions/all`

    ```go
    func DeleteAllSessions(context.Context) ([]string, error)
    ```

    
