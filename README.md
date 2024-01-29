# osi
# osi working model
import random

class Message:
    def __init__(self, data):
        self.data = data

class ApplicationLayer:
    def __init__(self, message):
        self.message = message

    def send(self):
        print("Application Layer: Sending message:", self.message.data)
        presentation_layer = PresentationLayer(self.message)
        presentation_layer.send()
   

class PresentationLayer:
    def __init__(self, message):
        self.message = message

    def send(self):
        print("Presentation Layer: Converting data to application format...")
        encoded_data = self.message.data.encode("utf-8")
        session_layer = SessionLayer(encoded_data)
        session_layer.send()

class SessionLayer:
    def __init__(self, data):
        self.data = data

    def send(self):
        print("Session Layer: Establishing session and adding header...")
        header = f"Session ID: {random.randint(1000, 9999)}"
        data_with_header = f"{header}\n{self.data}"
        transport_layer = TransportLayer(data_with_header)
        transport_layer.send()

class TransportLayer:
    def __init__(self, data):
        self.data = data

    def send(self):
        print("Transport Layer: Segmenting data into packets...")
        packets = []
        for i in range(0, len(self.data), 10):
            packets.append(self.data[i:i+10])
        network_layer = NetworkLayer(packets)
        network_layer.send()

class NetworkLayer:
    def __init__(self, packets):
        self.packets = packets

    def send(self):
        print("Network Layer: Adding routing information and calculating checksum...")
        for packet in self.packets:
            # Simulate calculating checksum
            checksum = sum(map(ord, packet)) % 256
            packet_with_checksum = f"{packet}\nChecksum: {checksum}"
            data_link_layer = DataLinkLayer(packet_with_checksum)
            data_link_layer.send(packet_with_checksum)

class DataLinkLayer:
    def __init__(self, data):
        self.data = data

    def send(self, data):
        print("Data Link Layer: Framing data and adding MAC addresses...")
        # Simulate adding MAC addresses
        source_mac = "AA:BB:CC:DD:EE:FF"
        destination_mac = "11:22:33:44:55:66"
        framed_data = f"{source_mac} -> {destination_mac}\n{self.data}"
        physical_layer = PhysicalLayer(framed_data)
        physical_layer.send()

class PhysicalLayer:
    def __init__(self, data):
        self.data = data

    def send(self):
        print("Physical Layer: Converting data to bits and transmitting...")
        # Simulate converting data to bits
        bits = ''.join(format(c, '08b') for c in self.data.encode("ascii"))
        print("Transmitted bits:", bits)

# Create a message and send it through the OSI layers
message = Message("Hi Bharath!")
application_layer = ApplicationLayer(message)
application_layer.send()
message = Message("Hello Sahith")
application_layer = ApplicationLayer(message)
application_layer.send()
