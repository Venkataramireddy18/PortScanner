import socket

target = input("Enter target IP address: ")

open_ports = []

print("-" * 50)
print(f"Scanning Target: {target}")
print("-" * 50)

for port in range(7990, 8010):

    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.settimeout(0.1)

    result = s.connect_ex((target, port))

    if result == 0:

        try:
            service = socket.getservbyport(port)
        except:
            service = "Unknown Service"

        print(f"[OPEN] Port {port} - {service}")

        open_ports.append((port, service))

    s.close()

# SAVE RESULTS
with open("results.txt", "w") as file:

    file.write(f"Scan Results for {target}\n")
    file.write("-" * 40 + "\n")

    for port, service in open_ports:
        file.write(f"Port {port} - {service}\n")

print("\nScan Completed.")
print("Results saved successfully.")
