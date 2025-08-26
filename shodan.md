
- Server: Hipcam RealServer has_screenshot:true
- nmap -p 554 --script rtsp-url-brute ip
- ffplay -rtsp_transport tcp -i rtsp://ip:554/11
- ffplay -rtsp_transport udp -i rtsp://ip:554/11
- ffplay -rtsp_transport tcp -i rtsp://admin:admin@SEU_IP:554/Streaming/Channels/101


