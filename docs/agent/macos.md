# Installing Mondoo Agent on macOS workstation

<img src="../assets/videos/mondoo-setup-macos.gif">

At first, add mondoo brew tap:

```
brew tap mondoolabs/mondoo
```

Then, install the mondoo agent:

```bash
brew install mondoo
```

Register the agent with your mondoo cloud organization

```bash
mondoo register --token 'TOKEN'
```
