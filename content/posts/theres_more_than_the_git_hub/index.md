---
title: "There's more than just the Hub"
date: 2024-09-20
draft: false
thumbnail: "github_logo.png"
thumbnailAlt: "Event photo"
tags: ["tech", "git", "automation", "configuration", ""]
---

{{< figure src="git_providers.jpg" >}}

GitHub’s treatment of the original maintainer of the XZ package was sub-optimal, the course of action was understandable but it left a bad after-taste. It got me thinking, what if my account with all my incredibly cool open source projects that definitely have many fans, were to be terminated?

Some context for those unaware of the XZ backdoor conundrum:

{{< youtube jqjtNDtbDNI >}}


I don’t want to lose my projects and badly written code, so, how do I make sure I have back-ups outside of my computer and GitHub?

I got around setting up an environment to make sure my projects have back-ups. First I need to have accounts in different providers so I created one in GitLab and another in BitBucket, but you could also add GitTea or OneDev for those more privacy inclined.

After creating the accounts, I recommend you create SSK keys associated with those providers, additionally you should create a standard key for commit signing.

**Note:** You can sign git commits with SSH keys but I still prefer suffering and so I use GPG keys. Choose whatever is best for you.

After adding each key to it’s respective provider, make sure you have your SSH config updated, for example:

```sh 
$cat ~/.ssh/config
Host gitlab.com
  User git
  AddKeysToAgent yes
  UseKeychain yes
  ControlMaster no
  IdentitiesOnly yes
  IdentityFile ~/.ssh/sillylab
Host github.com
  User git
  AddKeysToAgent yes
  UseKeychain yes
  ControlMaster no
  IdentitiesOnly yes
  IdentityFile ~/.ssh/fuckmicrosoft
Host bitbucket.org
  User git
  AddKeysToAgent yes
  UseKeychain yes
  ControlMaster no
  IdentitiesOnly yes
  IdentityFile ~/.ssh/jirahater9000
```

Unemployed people need not worry too much. Employed people or open source maintainers should however be aware of how easy it is to impersonate some on git: Article

If like me you want to sign your commits with a GPG key, you can follow the instructions for each provider in these links:

- Github: [link](https://docs.github.com/en/authentication/managing-commit-signature-verification/generating-a-new-gpg-key)
- Gitlab: [link](https://docs.gitlab.com/ee/user/project/repository/signed_commits/gpg.html)
- BitBucket: [link](https://confluence.atlassian.com/bitbucketserver/using-gpg-keys-913477014.html#UsingGPGkeys-AddaGPGkeytoBitbucketServer)

In order to simplify this process I’ll advise you article reader. Use the same GPG key for all these providers, it makes your life easier and collaborators can check if the signature is the same between providers and trust it is really you.

Now, all that’s missing is to understand how to push and pull to and from all the remote endpoints at once:

```sh
# Create a remote endpoint that serves as a cath-all, in this case it's 'all'
$git remote add all git@github.com:ElMassas/example.git
$git remote set-url --add --push all git@github.com:ElMassas/example.git
$git remote set-url --add --push all git@gitlab.com:ElMassas/example.git
$git remote set-url --add --push all git@bitbucket.org:ElMassas/example.git
```

To publish your code to all your remote endpoints all you need to now is:

```sh
$git push all "$(git_current_branch)"
```

You shouldn’t really need to worry much about pulling from the other providers if they are being used to store copies of your code, however:

```sh
$git fetch --all
```

That’s it.
