### Daily startup (iMac desktop node)

To bring this desktop node online:

1. Press Cmd+Space and open **Terminal**.
2. Run:

   ```bash
   cd ~
   ./start-mesh-driver.sh
   ```

3. Wait a few seconds, then open in your browser:

   - MeshChatX: http://127.0.0.1:8787
   - rBrowser:  http://127.0.0.1:5500

On this machine, `start-mesh-driver.sh` starts Reticulum (`rnsd`), MeshChatX and rBrowser.
