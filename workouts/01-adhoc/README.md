# Running commands

```
ansible -i hosts all -a "ip addr"
ansible -i hosts ourhost -m copy -a "src=test.txt dest=/tmp/test.txt"
ansible -i hosts ourhost -m setup
```

