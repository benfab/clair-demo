# clair-demo
Docker image scanning demo with Clair

# Requirements 

- CentOS 7 with Development tools installed `yum group install "Development Tools"`    
- Docker engine 
- Docker compose
- Golang 1.8 

# Deploy
Move in to the project directory and run docker-compose
`cd clair-demo`  
`docker-compose up -d`



# Install Clair

Create docker network     
`docker network create clair`

Deploy a Postgres instance  
`docker run -d -e POSTGRES_PASSWORD="" -p 5432:5432 --network clair --name postgres postgres:9.6`

Download the default config file for Clair  
`curl -L https://raw.githubusercontent.com/coreos/clair/master/config.example.yaml -o $HOME/clair_config/config.yaml`

Update the config file line 23 by changing the postgres host from localhost to postgres  
`vi $HOME/clair_config/config.yaml`

Deploy an instance of clair  
`docker run -d -p 6060-6061:6060-6061  -v /tmp:/tmp -v $HOME/clair_config:/config --network clair quay.io/coreos/clair-git:latest -config=/config/config.yaml`

 
Deploy a private repo  
`docker run -d -p 5000:5000 --restart=always --name registry registry:2`

# Install clairctl 

clairctl is a convenient CLI client to interact with the Clair API.  
See https://github.com/jgsqware/clairctl

Install Glide  
`curl https://glide.sh/get | sh`
`glide install -v`
`go build`

Build clairctl  
`export GOPATH=/usr/local/go/src/`  
`git clone https://github.com/jgsqware/clairctl  $GOPATH/src/github.com/jgsqware/clairctl`  
`cd $GOPATH/src/github.com/jgsqware/clairctl`  
`go build`

Copy the clairclt executable in the bin directory  
`cp -v $GOPATH/src/github.com/jgsqware/clairctl/clairctl /usr/local/bin`


# Generate an image scanning report with clairctl

Pull the official nginx Docker image from Docker Hub to Clair    
`clairctl pull nginx`    
Analyze the nginx Docker image    
`clairctl analyze nginx`  
Generate the report     
`clairctl report nginx`  
The HTML report is available here       
`$HOME/reports/html/analysis-nginx-latest.html`  


# Integrate the image scanning into Jenkins pipelines with klar

https://github.com/optiopay/klar

Install klar  
`curl -L https://github.com/optiopay/klar/releases/download/v1.2.1/klar-1.2.1-linux-amd64 -o /usr/local/bin/klar`  
`chmod +x  /usr/local/bin/klar`

# Notes
HTTPS registries are not part of this demo.
