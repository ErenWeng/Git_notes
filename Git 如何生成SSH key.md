# Git generate Secure Shell Key

>一般在登入伺服器時都是使用帳號密碼登入，每次登入都要輸入一次帳號密碼；但如果使用SSH key登入，就不用每次都手動輸入密碼登入，使用上不但增加了安全性，也方便許多。

## 產生SSH key步驟

1. 輸入`ssh-keygen` 產生key

2. 詢問要放置的位置，預設為`/home/username/.ssh/id_rsa`（最後面可能會有不同的伺服器名稱），直接按enter就是使用預設

    ```.
    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/username/.ssh/id_rsa):
    ```

3. 詢問要設定的短密碼，每次登入都要使用，當然也可以按2次enter不設定密碼（就是懶得用密碼...）

   ```.
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    ```

4. 生成key，就會看到.ssh資料夾中的私鑰`id_rsa`和公鑰`id_rsa.pub`

    ```.
    Your identification has been saved in /home/username/.ssh/id_rsa.
    Your public key has been saved in /home/username/.ssh/id_rsa.pub.
    The key fingerprint is:
    SHA256:gby9q8/zmEbBWTjTiPOY/E3yj1NAhs9W3u3tLKGlJPg Eren@demo.com
    The key's randomart image is:
    +---[RSA 3072]----+
    |       . =       |
    |     .o.* = .    |
    |     .o*.X o . . |
    |      +o*.* . . .|
    |      ..SO .   ..|
    |        +.+ o o o|
    |       ... * + + |
    |       .o+E + . o|
    |      .+*o..   . |
    +----[SHA256]-----+
    ```

5. 複製公鑰（小心不要複製到私鑰），輸入cat和公鑰位置 `cat ~/.ssh/id_rsa.pub` 將公鑰印出，然後複製下來，ssh-rsa也算是公鑰的內容

    ```.
    ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDtnTAk9Orh1b21BtXrkpGgcLO5uv+PmFWStoNumVUSDKzoaueA3ewnZHE2+PPUAnDN+X0Z5vFMuK2q9nbABRr9xTRR0tL7FW3yWzZX8EKDF1ck5ndTXagoLNQD+Grt7SSm820EW6qksgUVsqOYeFcE4ilxv8yCTPd6bpgWfEivQLPm/vp/wjNznX9hDOoFLwheOHG2AAF1PIZq3qxUlS2YyuLwj61sYstbp+onesnkOZ+WwAKz+e/hrj4KDlRIMHSfI59KzjOak3ijzc0AcyBYjWEBW229XT+SUypr6BOG2Aol98azl0a40v2sERSZgBJOzsj3gBEwCsh39D69jnS9b2l9HgQHMvTDXsQlRn57JVndwXHbWOvjO+WZ/CLKGJwTajz688YZfWBZ0o7+tjxaCMMqoOCtI0ToGjkQwAvyjHr5ddeF9AM+5Cd0wVx+UpHwVkQRUKynibGd49CyLueeAjiWR27bURwWtue4AlQ6ry7pQ3lWnWjzBgeFJGlVHXE= Eren@demo.com
    ```

6. 放到伺服器中，以我使用的伺服器GitHub為例，如果要在單一的repository使用，就進入資料庫的Settings中Deploy keys，按下Add deploy key，貼上Key，記得下面的Allow write access選項要記得打勾不然會不能pull。\
如果是使用在全部的repositoies，就直接進入個人的Settings按下New SSH key後新增即可。