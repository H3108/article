1. 拉取子模块代码
git submodule update --remote --merge src/app/jm
2. 推送子模块代码
cd src/app/jm
git add . && git commit -m "xxxxx" && git push
cd ../../../
git add . && git commit -m "src/app/jm commit" && git push
