# 🖥️ Task 7 — Monitor System Resources Using Netdata (Docker)

Monitor a system in real time using **Netdata**. This guide shows how to run Netdata with Docker, open the dashboard, explore metrics, and capture screenshots.

---

## 🎯 Objective

- Installed **Netdata** with **Docker**
- Viewed live **CPU, memory, disk, network**, and **Docker** metrics
- Explored **alerts** and **logs**
- Captured **screenshots** as deliverables

---

## ✅ Prerequisites

- Docker installed and running
- Port **19999** open on your machine/VM

---

## 🚀 Step by Step Explanation of what i have done 

Ran Netdata in Docker:

-- docker run -d --name netdata \
  -p 19999:19999 \
  --cap-add SYS_PTRACE \
  --security-opt apparmor=unconfined \
  netdata/netdata

Opened the dashboard through my ip:

-- http://192.168.1.9:19999

<img width="1891" height="997" alt="Screenshot 2025-08-14 123433" src="https://github.com/user-attachments/assets/74d2d97b-1f6a-4a6b-8e4b-8964396355dd" />
<img width="1912" height="652" alt="Screenshot 2025-08-13 200925" src="https://github.com/user-attachments/assets/94691524-9788-4c0d-94d1-00848473f9ad" />
<img width="1541" height="1002" alt="Screenshot 2025-08-14 124726" src="https://github.com/user-attachments/assets/c590ac60-47c9-46be-8d3e-d55071c51c3c" />


🧭 What i Explored on the Dashboard

System Overview → quick health view

CPU → per-core usage, soft/hard IRQs

Memory → RAM usage, swap

Disks → I/O, read/write, latency

Network → bandwidth per interface

Docker → container CPU, memory, I/O (if Docker containers are running)

Alerts → active alarms and thresholds

🧪 Verified It’s Running

-- docker ps

<img width="1716" height="148" alt="Screenshot 2025-08-14 124841" src="https://github.com/user-attachments/assets/9f7c151d-f6e9-452d-a142-51b850f1eb6c" />


Check health from logs:

-- docker logs --tail 50 netdata

<img width="1703" height="635" alt="Screenshot 2025-08-14 125449" src="https://github.com/user-attachments/assets/4decc6fd-0e5d-41ef-b5d5-08ace79c3799" />


📝 View Netdata Logs (inside container)

-- docker exec -it netdata bash

-- ls /var/log/netdata/

-- cat /var/log/netdata/error.log

-- cat /var/log/netdata/access.log

-- exit


⚡Generated Load to See Charts Move

-- docker run --rm alpine sh -c "apk add --no-cache stress-ng >/dev/null && stress-ng --cpu 2 --timeout 30s"

<img width="1686" height="282" alt="Screenshot 2025-08-14 124901" src="https://github.com/user-attachments/assets/64903218-5d66-4273-b623-da4e046ea65a" />


Refreshed the CPU charts while it runs.

🆘 Troubleshooting

Can’t open dashboard: Check port open (ss -tulpen | grep 19999), firewall rules, or cloud security groups.

No Docker charts: Ensure other containers are running, and you mounted -v /var/run/docker.sock:/var/run/docker.sock:ro.

SELinux/AppArmor issues: Keep --cap-add SYS_PTRACE and --security-opt apparmor=unconfined (already in the run command).
