# Kibana Installation on Ubuntu

This guide provides step-by-step instructions to install and configure **Kibana** on **Ubuntu**, assuming you have already installed **Java** and **Elasticsearch**.

---

## Prerequisites

Before proceeding, ensure the following are installed:

- [Java Installation Guide](https://github.com/Furkaragoz/big-data-ai-installation/tree/main/java)
- [Elasticsearch Installation]([Java Installation Guide](https://github.com/Furkaragoz/big-data-ai-installation/tree/main/elastic)

---

## Step 1 – Add Kibana GPG Key and Repository

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elastic-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/elastic-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list
```

---

## Step 2 – Install Kibana

```bash
sudo apt-get update
sudo apt-get install kibana
```

---

## Step 3 – Configure Kibana

Open the configuration file:

```bash
sudo nano /etc/kibana/kibana.yml
```

Modify or ensure the following lines are present:

```yaml
server.port: 5601
server.host: "0.0.0.0"
elasticsearch.hosts: ["http://localhost:9200"]
xpack.security.enabled: false
```

> Note: Security settings should be re-enabled for production use.

---

## Step 4 – Start & Enable Kibana

Start the Kibana service:

```bash
sudo systemctl start kibana
```

Enable Kibana on boot:

```bash
sudo systemctl enable kibana
```

Check the status:

```bash
sudo systemctl status kibana
```

---

## Step 5 – Access Kibana

Once Kibana is running, open your browser and visit:

```
http://<your-server-ip>:5601
```

You should see the Kibana UI.

---

## Notes

- This guide assumes Elasticsearch is running on `localhost:9200` with security disabled.
- Ensure that port `5601` is open on your firewall to access Kibana from the browser.
- For production deployments, enable security settings and configure proper TLS.

---

## Resources

- [Elasticsearch Guide](https://github.com/your-repo/elasticsearch-installation-guide)
- [Java Installation Guide](https://github.com/ozgunakin/java-installation-on-ubuntu20.04)
