[toc]
#prettier 格式化代码
1. 全局安装prettier 
 - `npm install -g prettier`
2. 在webstorm 中设置
 - `settings`——> `Tools`——> `External Tools` 中添加`Prettify` 点击应用
3. 使用git 的pre-commit(钩子函数)
 - 进入到项目根目录下的`.git/hooks`
 - 将`pre-commit.sample` 改名为`pre-commit` （使用`vm pre-commit.sample pre-commit`,将文件改为`pre-commit`,）
 - 如果删除`pre-commit.sample`  重新创建`pre-commit` 并添加内容，此时由于读写权限问题是设置的钩子函数无效，若想更改权限，使用命令`chmod 755 pre-commit` 便可将`pre-commit`的权限改成与`pre-commit.sample`及其他文件的权限改成一致，此时便可以使`pre-commit` 生效。
 - `vim pre-commit` 编辑`pre-commit` ，复制一下代码到`pre-commit` 
 
```
 #!/bin/sh
PATH="/usr/local/bin:$PATH"
 
jsfiles=$(git diff --cached --name-only --diff-filter=ACM "*.js" "*.jsx" "*.less" | tr '\n' ' ')
[ -z "$jsfiles" ] && exit 0
 
# Prettify all staged .js files
echo "$jsfiles" | xargs prettier --config .prettierrc --write
 
# Add back the modified/prettified files to staging
echo "$jsfiles" | xargs git add
 
exit 0
```
注：echo "$jsfiles" | xargs prettier --config  .prettierrc --write , 代表在文件中要有.prettierrc  的文件，里面定义了一些格式化的规则

4. 回到文件根目录，执行一下命令，更新index使`pre-commit`生效
 - `git update-index -g`
5. 设置完成，此时提交文件时如果文件有改动，在执行`git commit` 后会自动格式换代码
