# Registration for Openstack review site

오픈스택 개발을 하기 위해서 필수적인 등록 절차에 관한 문서입니다. 당 문서를 이해 하기 위해서는 아래 같은 지식이 필요합니다 

  - git 을 통한 소스 형상 관리 방법 이해 
  - linux에서 ssh 사용 

일반적인 github와 다르게 openstack 에서는 gerrit이라는 시스템을 이용하고 있습니다. 그리고 이것은 www.openstack.org 와 연계 되어 있습니다. 다음과 같은 절차를 거치면 source contribute 를 할 수 있습니다. 

#### 1.  Openstack Foundation 회원 등록 

openstack.org 의 [openstack.org/join] 에 가입을 합니다. 이때 Foundation Member로 가입합니다. 

#### 2. Ubuntu One 회원 등록 

Gerrit 을 사용하기 위해서 [Ubuntu One]에 가입합니다. 이때 주의 해야 할 것은, 위의 openstack.org에 가입한 정보와 동일 하게 정보 입력 해야 합니다. 

가입 후 이메일로 계정을 활성화 시킵니다. 

#### 3. Review.openstack 에 등록

[review 싸이트]에 접속해서 필요한 절차를 거쳐서 등록을 합니다. 

그리고 SSH키를 [ssh인증 탭]에서 등록 되었는지 확인하고, 추가적으로 [profile 탭] 에서 Username 이 등록되었는지 확인 합니다. 

#### 4. 접속 테스트 

git 과 git-review 를 설치 합니다.
``` 
#  yum install git git-review 
```
그리고 기본적인 git 설정을 합니다. 아래의 username들과 이메일  계정은 위의 openstack foundation의 정보와 일치 해야 합니다. 그리고 gitreview.username 이름이 위의 review 의 username과 일치 해야 합니다. 
``` 
git config --global --add gitreview.username "donghwi.cha"
git config --global --add user.name "donghwi cha"
git config --global --add user.email "donghwi.cha@gmail.com"
```
그리고 단축 경로도 저장합니다. 마찬가지로 이때도 User를 gerrit 과 일치 시킵니다. 
```
# vi ~/.ssh/config
Host review
        Hostname review.openstack.org
        Port 29418
        User donghwi.cha
```
설정이 완료되었으면 접속 테스트를 합니다. 이때 29418 포트로 통신 가능 해야 합니다. 아래의 두 명령어를 실행 했을 때 오류가 없어야 합니다. 
```
# ssh -p 29418 donghwi.cha@review.openstack.org
# git review -s

```
#### 5. Commit 및 Push 
소스를 수정하고, git status로 수정된 내용을 확인 후 commit 메세지를 생성합니다. 
```
# git commit -a --amend 
```
이때 다음과 같은 사항을 지켜야 합니다. 
1. 버그이면 Change-Id: 아랫줄에 다음을 입력
 * Closed-Bug: #1276088
2. 블루 프린트 일때
 * blueprint local-storage-volume-scheduling
3. 도큐먼트에 영향을 줄때
 * DocImpact Related to nova-network-objects
4. 보안 문제가 있을 때
 * SecurityImpact
5. 업그레이드에 영향을 미칠 때 (release notes 의 ‘Upgrade Notes’ section 수정 고려) 
 *  UpgradeImpact ..

push 를 하기 위해서 아래와 같이 명령어 실행 합니다. 
```
# git review
remote: Resolving deltas: 100% (5/5)
remote: Processing changes: new: 1, refs: 1, done
remote:
remote: New Changes:
remote:   https://review.openstack.org/98615
remote:
To ssh://donghwi.cha@review.openstack.org:29418/openstack/neutron.git
 * [new branch]      HEAD -> refs/publish/stable/icehouse/stable/icehouse
```


License
----
당 위키에 있는 모든 자료는 공개를 원칙으로 한 공유를 목표로 합니다. All the data in this git repository is published on GPL, with which we propose more public and trust worthy community.


[openstack.org/join]:http://openstack.org/join
[Ubuntu One]:https://login.launchpad.net/+login
[review 싸이트]:https://review.openstack.org/#/register/q/status:open,n,z 
[ssh인증 탭]:https://review.openstack.org/#/settings/ssh-keys
[profile 탭]:https://review.openstack.org/#/settings/
