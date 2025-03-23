# Elasticsearch Installation on Ubuntu

This README provides step-by-step instructions for installing and configuring **Elasticsearch 8.x** on **Ubuntu**.

---

## Java Installation
[Java Installation Guide](https://github.com/Furkaragoz/big-data-ai-installation/tree/main/java)

## Installation Steps

### 1. Import the Elasticsearch GPG Key
```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
```

### 2. Install `apt-transport-https` (if not already installed)
```bash
sudo apt-get install apt-transport-https
```

### 3. Add Elasticsearch Repository
```bash
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list
```

### 4. Update and Install Elasticsearch
```bash
sudo apt-get update && sudo apt-get install elasticsearch
```

---

## Configuration

Edit the Elasticsearch configuration file:

```bash
sudo nano /etc/elasticsearch/elasticsearch.yml
```

Update or add the following lines:

```yaml
cluster.name: fkaragoz
network.host: 0.0.0.0
http.port: 9200

xpack.security.enabled: false
xpack.security.enrollment.enabled: false

xpack.security.http.ssl.enabled: false
xpack.security.transport.ssl.enabled: false
```

---

## Start and Enable Elasticsearch

```bash
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch
```

To verify it's running:

```bash
curl http://localhost:9200
```

You should see basic cluster information in the response.

---

## Notes

- This setup disables security for ease of development/testing.
- For production environments, **it is highly recommended** to enable security features.



---

## Step 4 – Start & Enable Elasticsearch

Start the Elasticsearch service:
```bash
sudo systemctl start elasticsearch
```

Enable Elasticsearch to start on boot:
```bash
sudo systemctl enable elasticsearch
```

Check the service status:
```bash
service elasticsearch status
```

You should see output indicating that the service is active and running.

---

## Step 5 – Test Elasticsearch

Verify that Elasticsearch is running correctly by sending a request to the local server:
```bash
curl -X GET "http://localhost:9200/?pretty"
```

Expected output:
```json
{
  "name" : "your-node-name",
  "cluster_name" : "fkaragoz",
  "cluster_uuid" : "XXXXXXX",
  "version" : {
    "number" : "8.x.x",
    ...
  },
  "tagline" : "You Know, for Search"
}
```

This confirms that your Elasticsearch service is up and responding to requests.

