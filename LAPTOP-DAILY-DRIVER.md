### Daily startup (MacBook Air laptop node)

To bring this laptop node online:

1. Press Cmd+Space and open **Terminal**.
2. Run:

   ```bash
   cd ~
   ./start-mesh-driver.sh
   ```

3. After a few seconds, open in your browser:

   - MeshChatX: http://127.0.0.1:8787
   - rBrowser:  http://127.0.0.1:5500

On this machine, `start-mesh-driver.sh` starts Reticulum (`rnsd`), MeshChatX and the Air-specific rBrowser script (`start-rbrowser-air.sh`).
