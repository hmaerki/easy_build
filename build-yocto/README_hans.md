cd easy_build/build-yocto
cp -r ~/.ssh .
docker build -t build-yocto . && docker run -ti build-yocto

eval "$(ssh-agent -s)"
ssh-add
