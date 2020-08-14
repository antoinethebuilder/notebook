# CLI

## AWS-CLI SSO Configuration

### Automatic configuration

Becoming a super hero is a fairly straight forward process:

```
> aws configure sso

SSO start URL [None]: [None]: https://my-sso-portal.awsapps.com/start
SSO region [None]:us-east-1
```

{% hint style="info" %}
 The CLI tool will guide you to log in the desired AWS Account. Additional details [here](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sso.html).
{% endhint %}

## SSH/SCP with System Manager

### Requirements

* [SSM Agent](https://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent.html) on the target instance\(s\)
* [IAM permissions](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-getting-started-instance-profile.html) \(_which will not be discussed here_\)
* [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) on your host
* [SSM Plugin](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-install-plugin.html) on your host

### SSH Configuration

Add the following to the SSH config file:

{% tabs %}
{% tab title="Linux" %}
{% code title="$HOME/.ssh/config" %}
```bash
# SSH over Session Manager

host i-* mi-*

ProxyCommand sh -c "aws ssm start-session --target %h --document-name AWS-StartSSHSession --parameters 'portNumber=%p'"
```
{% endcode %}
{% endtab %}

{% tab title="Windows" %}
{% code title="%USERPROFILE%/.ssh/config" %}
```text
# SSH over Session Manager

host i-* mi-*

ProxyCommand C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe "aws ssm start-session --target %h --document-name AWS-StartSSHSession --parameters portNumber=%p"
```
{% endcode %}
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
If you are currently using a profile with AWS CLI, append `--profile MyProfile` to the ProxyCommand.
{% endhint %}

After this step, you will be able to use both SSH and SCP commands as usually.

```text
ssh -i dev.key ec2-user@i-0ba3d05e2b6c0fb36
```

```text
scp /Users/abi/bugfix.tar.gz ec2-user@i-0ba3d05e2b6c0fb36:/dir/file.tar.gz
```

{% hint style="info" %}
The username may vary depending on your configuration.
{% endhint %}

