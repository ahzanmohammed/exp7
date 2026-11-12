
import socket
import threading

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind(("0.0.0.0", 8080))
s.listen()

clients = []

def handle_client(client):
    while True:
        try:
            msg = client.recv(1024).decode()

            if not msg:
                break

            for c in clients:
                if c != client:
                    c.send(msg.encode())

        except:
            break

    clients.remove(client)
    client.close()

while True:
    client, addr = s.accept()
    clients.append(client)

    threading.Thread(
        target=handle_client,
        args=(client,)
    ).start()




import socket
import threading

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

server_ip = input("Enter server IP: ")
s.connect((server_ip, 8080))

name = "blessing"

def receive():
    while True:
        try:
            msg = s.recv(1024).decode()
            if msg:
                print(msg)
        except:
            break

threading.Thread(target=receive, daemon=True).start()

while True:
    message = input()

    if message.lower() == "exit":
        break

    s.send((name + ": " + message).encode())

s.close()



