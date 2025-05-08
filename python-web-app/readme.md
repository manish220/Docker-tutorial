
### 🧠 **Simple Explanation: ENTRYPOINT vs CMD in Docker**

Both `ENTRYPOINT` and `CMD` are instructions in a Dockerfile that define **what runs** when you start a container.

| Feature      | `ENTRYPOINT`                                | `CMD`                                            |
| ------------ | ------------------------------------------- | ------------------------------------------------ |
| Purpose      | Defines the **main command** to always run  | Provides **default arguments** to the entrypoint |
| Overridable? | Harder to override (unless explicitly done) | Easy to override when running `docker run`       |
| Best for?    | Apps with a **fixed start behavior**        | Apps where you may want to **change args**       |

---

### 📦 **Real-World Analogy**

Imagine you're ordering food:

* `ENTRYPOINT` is **always** the restaurant you order from.
* `CMD` is the **default meal** they suggest (you can override it if you want a different dish).

So if the restaurant is set to serve pizza (`ENTRYPOINT`), it might default to Margherita (`CMD`), but you can change it to Pepperoni when ordering.

---

### 🧪 **Dockerfile Example**

```dockerfile
FROM ubuntu

# Install curl
RUN apt-get update && apt-get install -y curl

# Set ENTRYPOINT as the main command
ENTRYPOINT ["curl"]

# Set CMD as the default argument
CMD ["https://example.com"]
```

---

### 🔍 What happens when you run the container?

```bash
docker build -t my-curl .
docker run my-curl
```

➡️ This will run:

```bash
curl https://example.com
```

You can **override CMD** like this:

```bash
docker run my-curl https://openai.com
```

➡️ Now it runs:

```bash
curl https://openai.com
```

But if you try to override `ENTRYPOINT`, you must use `--entrypoint`:

```bash
docker run --entrypoint ls my-curl -l
```

➡️ This ignores `curl` and runs `ls -l` instead.

---

### ✅ Summary:

* Use **ENTRYPOINT** when you always want the container to behave like a specific app.
* Use **CMD** to supply **default parameters** to that app, which you can change at runtime.

Would you like me to generate a diagram showing this flow?
