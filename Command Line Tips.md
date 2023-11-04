
## Important commands
[How to add bash auto completion in Ubuntu Linux - nixCraft (cyberciti.biz)](https://www.cyberciti.biz/faq/add-bash-auto-completion-in-ubuntu-linux/)
**set aliases**
`alias k=kubectl`
`alias ks=kubeadm`
`alias e=etcdctl`

**setting ns and ctx**
`kubectl config set-context <ctx> --namespace`
`kubectl config use-context <ctx>`

**don't wait**
`kubectl delete pod nginx --force`

**use grep**
`k desc pods | grep -C 10 "authour=Some Bloke`

**help**
`k <command> --help`
`k explain <resource>.<jsonpath>`

