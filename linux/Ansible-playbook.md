

Ansible play book test 脚本：

	-  将当前目录下的./test20210609.txt 复制到主机47.102.98.58 的/tmp 目录下

```
#!/usr/bin/env ansible-playbook
- hosts:
    - 47.102.98.58
  gather_facts: False
  become: True

  tasks:
    - name: copy config
      copy:
        src: ./test20210609.txt
        dest: /tmp
      register: update_config
```

