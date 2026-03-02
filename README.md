<p align="center">
  <a href="https://kevinanielsen.github.io/go-fast-cdn/" target="_blank" rel="noopener">
    <img src="https://i.imgur.com/zVNCMfV.png" alt="Go-Fast CDN - Fast and simple Open Source CDN" />
  </a>
  <a href="https://github.com/kevinanielsen/go-fast-cdn/actions/workflows/release.yml/" target="_blank" rel="noopener">
    <img src="https://github.com/kevinanielsen/go-fast-cdn/actions/workflows/release.yml/badge.svg?branch=main" />
  </a>
  <a href="https://goreportcard.com/report/github.com/kevinanielsen/go-fast-cdn" target="_blank" rel="noopener">
    <img src="https://goreportcard.com/badge/github.com/kevinanielsen/go-fast-cdn" />
  </a>
</p>

# Go-Fast CDN

_"The PocketBase of CDNs" - Me_

### A fast and easy-to-use CDN, built with Go.

Utilizing an SQLite database with GORM and the Gin web framework. UI built with [Vite](https://vite.js/) + [React](https://react.dev/) and [wouter](https://github.com/molefrog/wouter).

## How to use

See our documentation at [kevinanielsen.github.io/go-fast-cdn/](https://kevinanielsen.github.io/go-fast-cdn/)

## Community

Join the [discord](https://discord.gg/z9uqNtU6yS) to talk to fellow users and contributors!

## Building the Binary from Source File

1. Run `make prep` # Install Dependencies
2. Run `make clean` # Clean the Output Files (don't use if First Time Building)
3. Run `make test` # Ensure Code Quality, Verify Correct Functionality, Check Dependencies and Check Bugs in Code.  
4. Run `make build` # Build the Binary for Current Architecture 

**Build for Other Architectures:**<br>
Linux:`make build-linux`<br>
MacOS: `make build-darwin`<br>
Windows: `make build-windows`

This will Cross-Compile to Windows, MacOS(Darwin), and Linux Binaries, so make sure that you have the **MAKE** Package Installed which Installs the Compilers on your Machine if you run `make build`.<br>
<br>
If you don’t have the **MAKE** Compiler Installed you can use **GOLANG-GO**:<br>
<br>
Debian: `apt update && apt install golang-go -y`<br>
CentOS/Rocky: `dnf update && dnf install golang -y`<br>
Alpine: `apk update && apk add golang-go`<br>

Verify GO Version:<br>
`go version`

Then Run:<br>
`go build .`

## Development - Create Bare Metal, VM or LXC

### Clone the Repository

`git clone git@github.com:kevinanielsen/go-fast-cdn`
or `git clone https://github.com/kevinanielsen/go-fast-cdn`

### Move the Source File to a Global Directory or Custom Directory
Global Directory: 
<br>Linux: `/usr/local/bin/go-fast-cdn-linux`<br>
Windows: `\<PATH>\go-fast-cdn-windows` - NOTE: Need to Add Path to Windows Evironment Varbles<br> 
MacOS: `/usr/local/bin/go-fast-cdn-darwin`<br>

Custom Directory - Non Global:
<br>Linux: `/<PATH>/bin/go-fast-cdn-linux`<br>
Windows: `\<PATH>\go-fast-cdn-windows`<br>
MacOS: `/usr/local/bin/go-fast-cdn-darwin`<br>

### Add ENV Variables

This project uses [dotenv](https://vault.dotenv.org/) and I recommend that you do the same. <br>
Read more about the usage on their page. <br><br>
Once you’ve cloned the repo, you need to set up the Environment Variables.

Copy the .env.example file to a new .env file: `cp .env.example .env`

If you do not wish to use this, you can just rename `.example.env` to `.env` and fill in the fields.

### Make Executable for Linux and MacOs<br>

Linux: `chmod +x /bin/go-fast-cdn-linux`<br>
MacOS: `chmod +x /bin/go-fast-cdn-darwin`<br>


### Run Binary
Globally:<br>
Linux: `go-fast-cdn-linux`<br>
Windows:`Start Run go-fast-cdn-windows`<br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`CMD Prompt go-fast-cdn-windows`<br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `PowerShell Prompt go-fast-cdn-windows`<br>
MacOS: `go-fast-cdn-darwin`<br>

Non Global - Custom Directory:<br> 
Linux: `./<PATH>/go-fast-cdn-linux`<br>
Windows: `\<PATH>\go-fast-cdn-windows`<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `Create a Short Cut to go-fast-cdn-windows.exe`<br>         
MacOS: `./<PATH>/go-fast-cdn-darwin`<br>

### Create System Service File for Linux - Start Go-Fast-CDN Automatically on System Boot

Create File: `/etc/systemd/system/go-fast-cdn.service`

```
[Unit]
Description=Go-Fast CDN
After=network.target

[Service]
ExecStart=/<PATH>/<BINARY DIRECOTORY>/go-fast-cdn-linux
WorkingDirectory=/<PATH>/<BINARY DIRECOTORY>
Restart=always
User=root
Group=root

[Install]
WantedBy=multi-user.target
```

### Enable and Start Service:<br>
Enable Service: `systemctl enable go-fast-cdn`<br>
Start Service: `systemctl start go-fast-cdn`<br>
### Enable Stop Service:<br>
Stop Service: `systemctl stop go-fast-cdn`<br>
### Check Service Service:<br>
Check Service Status: `systemctl status go-fast-cdn`<br>

## Quick Start with Docker

`git clone git@github.com:kevinanielsen/go-fast-cdn`
or `git clone https://github.com/kevinanielsen/go-fast-cdn`

```
docker-compose up -d
```

## Subdomain Separation (Optional)

For production deployments, you can separate admin UI from public CDN serving using subdomains.

**Nginx**: See `nginx/example.conf` for a basic configuration.

**Nginx Proxy Manager**:
1. Create proxy host for `admin.cdn.example.com` → `http://your-cdn:8080` (full access)
2. Create proxy host for `cdn.example.com` → `http://your-cdn:8080`
3. Edit the `cdn.example.com` proxy host → **Custom Locations** tab → Add:
   - Location: `/api/cdn/download/images/` → Forward to `http://your-cdn:8080`
   - Location: `/api/cdn/download/docs/` → Forward to `http://your-cdn:8080`
4. Click the **gear icon ⚙️** (top-right) → In **Custom Nginx Configuration**, add:
   ```
   location / {
       return 403;
   }
   ```
5. Save. This blocks admin UI access on `cdn.example.com`
