# usva-go

`usva-go`-repository is a wrapper for https://github.com/usvacloud/usva REST-API.



## Supported features

- [ ] API endpoint support
  - [ ] `/` - General
    - [ ] `/restrictions` - Get different API restrictions
  - [ ] `/file` - Files 
    - [ ] `GET /info?filename=<uuid>` - Get file metadata 
    - [ ] `GET /?filename=<uuid>` - Download a file
    - [ ] `POST /upload` - Upload a file
  - [ ] `/account` - Accounts
    - [ ] `GET /profile` - Get current account
    - [ ] `POST /create` - Create a new accoutn
  - [ ] `/feedback` - Probably not going to be supported due to irrelevancy
- [ ] Wrapper features
  - [ ] Session persisting support
  - [ ] Controlled execution with `context` package
