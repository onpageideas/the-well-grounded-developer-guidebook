
# Guide (macOS): setup [puma-dev](https://github.com/puma/puma-dev) and use [as https proxy](https://github.com/puma/puma-dev#proxy-support)

Install puma-dev as [described in the official documentation](https://github.com/puma/puma-dev#installation)

```
brew install puma/puma/puma-dev


# Configure some DNS settings that have to be done as root
sudo puma-dev -setup
# Configure puma-dev to run in the background on ports 80 and 443 with the domain `.test`.
puma-dev -install
```

### Use [as https proxy](https://github.com/puma/puma-dev#proxy-support) 

To access via https for a server running on `localhost:3000` (like rails default):

```
echo '3000' > ~/.puma-dev/proxy3000

# "proxy3000" can be replaced with whatever anything else
```

Open browser via https to:  https://proxy3000.test

PS: the naming `proxy3000` is a made-up convention. You can use whatever name you want, but best practice is to adhere to a convention.

 
