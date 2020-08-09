# Start the cluster

{% hint style="info" %}
After bootstrapping, the only thing left to do is to either start or restart the cluster!
{% endhint %}

If you used the **exact** same docker-compose command mentioned above, you can press "_ctrl+c_" to stop the containers.

If you are running in "_detached mode_", run:

```bash
docker-compose restart
```

If the cluster is already did bootstrap and the containers are stopped, simply run:

```bash
docker-compose up -d
```

{% hint style="success" %}
Your cluster should be all set!
{% endhint %}



