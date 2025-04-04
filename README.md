# 📌 New Relic Integration with Spring Boot

## 📝 Summary

This document outlines how to integrate **New Relic** with a **Spring Boot** application. New Relic is a powerful application performance monitoring (APM) tool that enables real-time visibility into your app’s performance, including response times, throughput, and errors. Integration is achieved via the New Relic Java Agent, which collects data and sends it to New Relic’s APM platform.

---

## 📚 Table of Contents

1. [Requirements](#1-requirements)  
2. [Create a New Relic Account and Get the License Key](#2-create-a-new-relic-account-and-get-the-license-key)  
3. [Download the New Relic Java Agent](#3-download-the-new-relic-java-agent)  
4. [Set Up New Relic in the Spring Boot Project](#4-set-up-new-relic-in-the-spring-boot-project)  
5. [Configure New Relic with License Key](#5-configure-new-relic-with-license-key)  
6. [Add the New Relic Agent to Spring Boot Startup](#6-add-the-new-relic-agent-to-spring-boot-startup)  
7. [Update Spring Boot Configuration](#7-update-spring-boot-configuration)  
8. [Set Up `.env` for Credentials](#8-set-up-env-for-credentials)  
9. [Test New Relic Setup](#9-test-new-relic-setup)  
10. [Screenshots](#10-screenshots)  
11. [Troubleshooting](#11-troubleshooting)  

---

### 1. Requirements

Ensure the following prerequisites are met:

- A working Spring Boot application.
- Java 8 or higher.
- A New Relic account – [Sign Up Here](https://newrelic.com/).
- Access to your New Relic License Key.

---

### 2. Create a New Relic Account and Get the License Key

1. **Sign up** at [New Relic](https://newrelic.com/signup).  
2. **Log in** to your New Relic dashboard.  
3. Navigate to **Account Settings → License Key** to obtain your key.  
4. Copy the key – you’ll use it in your configuration.

---

### 3. Download the New Relic Java Agent

1. Visit [Java Agent Documentation](https://docs.newrelic.com/docs/infrastructure/infrastructure-agent/windows-installation/windows-msi-installer/).  
2. Download and extract the `newrelic-agent.zip`.  
3. Place the extracted `newrelic` folder in your project root.

---

### 4. Set Up New Relic in the Spring Boot Project

1. Copy the `newrelic` directory (contains `newrelic.jar` and `newrelic.yml`) to your app’s root.  
2. Edit `newrelic.yml`:  
   - Set `app_name` to the desired application name.  
   - This name will appear in the New Relic dashboard.

---

### 5. Configure New Relic with License Key

Update `newrelic.yml`:

```yaml
license_key: 'your_new_relic_license_key'
app_name: 'MySpringBootApp'
```

You can also use environment variables:

```yaml
license_key: <%= ENV["NEW_RELIC_LICENSE_KEY"] %>
```

---

### 6. Add the New Relic Agent to Spring Boot Startup

Use the `javaagent` flag in your application startup command:

```bash
java -javaagent:/path/to/newrelic.jar -jar target/your-app.jar
```

Replace `/path/to/newrelic.jar` with the actual path.

If you're building with Maven or Gradle, ensure the agent is included when packaging or running the app.

---

### 7. Update Spring Boot Configuration

Update either `application.properties` **or** `application.yml` for New Relic metrics export:

#### application.properties

```properties
management.newrelic.metrics.export.enabled=true
management.newrelic.metrics.export.api-key=${NEW_RELIC_LICENSE_KEY}
management.newrelic.metrics.export.account-id=${NEW_RELIC_ACCOUNT_ID}
management.newrelic.metrics.export.uri=https://insights-collector.newrelic.com/v1/accounts/${NEW_RELIC_ACCOUNT_ID}/events
management.newrelic.metrics.export.step=60s
logging.level.io.micrometer.newrelic=DEBUG
```

#### application.yml

```yaml
management:
  newrelic:
    metrics:
      export:
        enabled: true
        api-key: ${NEW_RELIC_LICENSE_KEY}
        account-id: ${NEW_RELIC_ACCOUNT_ID}
        uri: https://insights-collector.newrelic.com/v1/accounts/${NEW_RELIC_ACCOUNT_ID}/events
        step: 60s
logging:
  level:
    io:
      micrometer:
        newrelic: DEBUG
```

---

### 8. Set Up `.env` for Credentials

Create a `.env` file at the project root:

```env
NEW_RELIC_LICENSE_KEY=your_license_key_here
NEW_RELIC_ACCOUNT_ID=your_account_id_here
```

Add `.env` to `.gitignore`:

```bash
# .gitignore
.env
```

If you want Spring Boot to automatically read the `.env` file, add the following Maven dependency:

```xml
<dependency>
  <groupId>me.paulschwarz</groupId>
  <artifactId>spring-dotenv</artifactId>
  <version>3.0.0</version>
</dependency>
```

Or set the variables in your system environment.

---

### 9. Test New Relic Setup

1. **Start the Application** using Maven or the `.jar` file.  
2. Log in to [New Relic Dashboard](https://one.newrelic.com/) and open **APM**.  
3. Select your app from the list.  
4. Confirm metrics like response time, throughput, and JVM stats are visible.  
5. You can also push **custom metrics** or **events** via API for validation.

---

### 10. 📸 Screenshots

📊 **Summary**  
![Summary](/assets/summary1.png)  

![Summary](/assets/summary2.png)  

![Summary](/assets/summary3.png) 

🧩 **Service Map**  
![Service Map](/assets/servicemap.png)  

⚙️ **Transactions**  
![Transactions](/assets/transactions.png)  

🗄️ **Database**  
![Database](/assets/database.png)  

🔥 **JVM**  
![JVM](/assets/jvm.png)  

🧾 **Logs**  
![Logs](/assets/logs.png)  

---

### 11. Troubleshooting

- ✅ **Check License Key**: Ensure it's valid in both `.env` and `newrelic.yml`.  
- 📂 **Correct Agent Path**: Verify the `javaagent` path in the startup command.  
- 🌐 **Network Access**: Ensure your environment allows outbound access to New Relic.  
- 🪵 **Check Logs**: Review `newrelic.log` inside the `newrelic` folder for details.  
- 📖 **Documentation**: Refer to the [New Relic Docs](https://docs.newrelic.com/) for additional help.

---

## ✅ Conclusion

By following this guide, you have successfully integrated New Relic with your Spring Boot application. You can now monitor your app’s behavior in real time, analyze performance bottlenecks, and improve reliability using actionable insights from the New Relic APM platform.

---
