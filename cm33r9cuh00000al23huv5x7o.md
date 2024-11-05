---
title: "Kubernetes Cheat Sheet"
seoTitle: "Kubernetes Quick Reference Guide"
seoDescription: "Kubernetes commands cheat sheet for setup, preparation, and management, with aliases. Periodically updated"
datePublished: Tue Nov 05 2024 01:12:21 GMT+0000 (Coordinated Universal Time)
cuid: cm33r9cuh00000al23huv5x7o
slug: kubernetes-cheat-sheet
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/jOqJbvo1P9g/upload/c3e257c8a95e6717ce4c652a974da271.jpeg
tags: kubernetes, devops, cheatsheet

---

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Most commands used here are also usable in OpenShift</div>
</div>

This cheat sheet summarizes my most commonly used Kubernetes commands, and I will update it periodically.

### Prepare prompt

```bash
#! ~/.bashrc

export KUBE_EDITOR="nano" # change default editor to nano
export KUBECONFIG=$(find "${HOME}/.kube/" -name "*.yaml" -printf "%p:" | sed 's/:$//')

# Disable the default virtualenv prompt change
export VIRTUAL_ENV_DISABLE_PROMPT=1

# Define a function to display the Git branch
parse_git_branch() {
    local branch
    branch=$(git branch 2>/dev/null | sed -n '/^\*/s/^\* //p')
    if [ -n "$branch" ]; then
        echo "($branch)"
    fi
}

# Define a function to display the Kubernetes context
kube_context() {
    kubectl config current-context 2>/dev/null
}

# Define a function to display virtual environment information
venv_info() {
    if [ -n "$VIRTUAL_ENV" ]; then
        echo " ($(basename "$VIRTUAL_ENV"))"
    fi
}

# Function to update the PS1 prompt dynamically
update_prompt() {
    PS1="\[\033[01;33m\]$(parse_git_branch)\[\033[01;34m\]\[\033[01;33m\](\$(kube_context))\[\033[00m\]\[\033[01;32m\]\u\[\033[00m\] \w\[\033[00m\]\[\033[01;36m\]$(venv_info)\[\033[00m\] $ "
}

# Set PROMPT_COMMAND to call the update_prompt function before displaying the prompt
PROMPT_COMMAND=update_prompt 
source <(kubectl completion bash)
```

### Prepare aliases

```bash
#! ~/.bash_aliases

alias reload='source ~/.bashrc'

# k8s
alias k='kubectl'
alias ka='kubectl apply --recursive -f'
alias kgn='kubectl get nodes -o wide'
alias kgns='kubectl get namespaces'
alias kgp='kubectl get pods -o wide'
alias kgd='kubectl get deployment -o wide'
alias kgs='kubectl get svc -o wide'
alias kge='kubectl get events --sort-by=.metadata.creationTimestamp'

alias kd='kubectl describe'
alias kdp='kubectl describe pod'
alias kdd='kubectl describe deployment'
alias kds='kubectl describe service'
alias kdn='kubectl describe node'

alias krm='kubectl delete'
alias kex='kubectl exec -i -t'
alias klog='kubectl logs -f'
alias krmf='kubectl delete -f'

alias kubens='kubectl config set-context --current --namespace '
alias kubectx='kubectl config use-context '
alias switch='kubectl config use-context '
alias kubegc='kubectl config get-contexts'
alias krew='kubectl krew'

alias ceph-tool='kubectl -n rook-ceph exec -it $(kubectl -n rook-ceph get pod -l 'app=rook-ceph-tools' -o jsonpath='{.items[0].metadata.name}') -- bash'

alias rm-oidc-token='rm -r ~/.kube/cache/oidc-login'
```

### Setup

```bash
kubectl config view # View kubeconfig settings
kubectl config use-context <context-name> # Switch to a specific context
```

### What’s next?

All the aliases provided above should be sufficient for daily use of Kubernetes.