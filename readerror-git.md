# read error GIT
---
이 문서는 git error 해결을 위해 작성하는 문서이다.

## Permission Error
현재 발생하는 에러는 다음과 같다.
```
git add.
git commit -m "chord(maintain)"
git push -u origin master

remote: Permission to tadkim/MarineAccidentViewer.git denied to stuc
fatal: unable to access 'https://github.com/tadkim/MarineAccidentVie
```
현재 github 계정을 개인계정(tadkim)과 스튜디오계정(stuckyi) 이렇게 두 개를 사용중인데, 개인프로젝트를 작성하고 개인계정(tadkim)의 remote repository에 `git remote add`후 `add`와 `commit`을 거쳐 `push`하려는 순간, `permission denied`라는 에러메시지가 나타났다.



## 해결방법 찾기
1. git config에서 사용자 수정
일단 내생각에 두 가지 계정을 사용하면서 sshkey인가를 변경했던 것 같다. 아마 이게 별도의 로그인없이 사용자 컴퓨터에서 몇가지 인증절차를 거치면 자동으로 다음부터는 그 git에서 '이 사용자가 그 사용자'임을 기억해서 로그인없이 `commit`, `push`를 날릴 수 있는 권한을 주었던 것이다.
- [참고 사이트 git-scm.com](https://git-scm.com/book/ko/v1/%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-Git-%EC%B5%9C%EC%B4%88-%EC%84%A4%EC%A0%95)

> 덧붙여 말하자면, 처음에는 웹브라우저상에서 '자동로그인'되어있는 계정이 `push`하고자 하는 계정이 아닐경우 `permission denied` 에러가 발생하는 줄 알았다. 하지만, 원하는 계정으로 로그인 후 다시 해도 안되는건 마찬가지였다. 결국 근본적인 문제를 찾아야하는 듯!

'근본적인 문제'를 찾아본다고 하지만, 사실 하나씩 해볼 수 밖에 없다. 기본적으로 git이 알고있는 나의 정보를 stuckyi에서 tadkim으로 변경해주어야할 것 같다. (스스로 할 줄 모른다고 했으니) 

다음은 git 환경에 대해 사용자이름(user.name)과 사용자 이메일주소(user.email)을 수정해주는 부분이다.

```
bash-3.2$ git config --global user.name "tadkim"                
bash-3.2$ git config --global user.email kor.tgkim@gmail.com
```

방금 수정한 내용을 포함하여 현재 내가 사용하고 있는 환경에서의 git의  모든 configuration 설정 내용을 출력 해본다.
- `git config --list` : 설정한 모든 내용을 보여준다.
```
bash-3.2$ git config --list
// git config --list의 결과
// credential.helper=osxkeychain                                       
// filter.lfs.clean=git lfs clean %f                               
// filter.lfs.smudge=git lfs smudge %f                             
// filter.lfs.required=true                                            
// user.name=tadkim                                                
// user.email=kor.tgkim@gmail.com                                      
// core.editor=subl -n -w                                          
// core.autocrlf=input                                             
// core.excludesfile=/Users/admin/.gitignore_global                    
// push.default=simple                                                 
// difftool.sourcetree.cmd=opendiff "$LOCAL" "$REMOTE"             
// difftool.sourcetree.path=                                           
// mergetool.sourcetree.cmd=/Users/admin/Applications/SourceTree.app/Co
// ntents/Resources/opendiff-w.sh "$LOCAL" "$REMOTE" -ancestor "$BASE" 
// -merge "$MERGED"                                                    
// mergetool.sourcetree.trustexitcode=true                             
// core.repositoryformatversion=0                                      
// core.filemode=true                                                  
// core.bare=false                                                     
// core.logallrefupdates=true                                          
// core.ignorecase=true                                                
// core.precomposeunicode=true                                         
// remote.marine.url=https://github.com/tadkim/MarineAccidentViewer.git
// remote.marine.fetch=+refs/heads/*:refs/remotes/marine/*          
```

git configuration의 모든 부분이 아닌 일부분도 따로 출력해서 확인할 수 있다.
- git config {key}
```
bash-3.2$ git config user.name
//tadkim
```
여기까지 마치고 위에서와 동일하게 `add`, `commit`, `push`까지의 코드를 작성해봤다. 결과는 다음과 같다.
```
bash-3.2$ git push marine -u master                                                               
remote: Permission to tadkim/MarineAccidentViewer.git denied to stuckyi.                          
fatal: unable to access 'https://github.com/tadkim/MarineAccidentViewer.git/': The requested URL r
eturned error: 403 
```
