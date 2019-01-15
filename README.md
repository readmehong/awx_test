# awx_test


* ansible hosts 관리  
  * /etc/ansible/hosts



* HP 장비느 보안상의 이유로 다이렉트로 접속으 하여도 모든 기능으 사용할 수 없다고 함.
  * 관련 내용 : https://cigol.tistory.com/1194
  * display 등의 명령어를 사용하기 위해선 접속 후 `_cmdline-mode on` 입력하여 명령어 사용 가능한 상태로 만들어야 함.
   * password를 입력하라 나오는데, CLI type the code 를 입력해야함.
   * 아래 버전으로는 현 패스워드가 뭔지 모름 
 ``` 
  ******************************************************************************
* Copyright (c) 2010-2016 Hewlett Packard Enterprise Development LP          *
* Without the owner's prior written consent,                                 *
* no decompiling or reverse-engineering shall be allowed.                    *
*****************************************************************************
```

* 아래는 `_cmdline-mode on` 명령어로 awx에서 실행해본 결과. 명령어가 없다고 나옴

  * 테스트 yml
   ```
   1 ---
  2 - name: Hello World Sample
  3   hosts: all
  4   #connection: netconf
  5 #  connection: lxc
  6   connection: local
  7   remote_user: admin
  8   tasks:
  9     - name: TEST
 10       expect:
 11         echo: yes
 12         command: _cmdline-mode on
 13         responses:
 14           'All commands can be displayed and executed. Continue? [Y/N]': "y\n"
 15           'Enter current password for root (enter for none):': "\n"
 ``` 
 * 실행 결과 log
   ``` 
The full traceback is:
41
Traceback (most recent call last):
42
  File "/tmp/ansible_expect_payload_HhilPE/__main__.py", line 193, in main
43
    encoding='utf-8')
44
  File "/var/lib/awx/venv/ansible/lib/python2.7/site-packages/pexpect/run.py", line 100, in run
45
    cwd=cwd, env=env, **kwargs)
46
  File "/var/lib/awx/venv/ansible/lib/python2.7/site-packages/pexpect/pty_spawn.py", line 204, in __init__
47
    self._spawn(command, args, preexec_fn, dimensions)
48
  File "/var/lib/awx/venv/ansible/lib/python2.7/site-packages/pexpect/pty_spawn.py", line 276, in _spawn
49
    'executable: %s.' % self.command)
50
ExceptionPexpect: The command was not found or was not executable: _cmdline-mode.
51
52
fatal: [192.168.101.60]: FAILED! => {
53
    "changed": false, 
54
    "invocation": {
55
        "module_args": {
56
            "chdir": null, 
57
       …
...
70
71
PLAY RECAP *********************************************************************
13:45:34
72
192.168.101.60             : ok=1    changed=0    unreachable=0    failed=1   

 ``` 
