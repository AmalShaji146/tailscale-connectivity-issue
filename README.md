Incident Title
Tailscale SSH connectivity failure due to offline Windows node

Affected Systems
Client: MSI Windows PC
Target: Ubuntu server skynet-1

Symptoms
Unable to SSH from Client to Server over Tailscale
tailscale status on Server showed:
	msi windows offline
Tailscale GUI on Client failed to open

Root Cause
Tailscale client on Windows entered an offline/disconnected state due to
stale authentication/session state or failed background service communication.

Diagnosis Steps
Checked tailscale status on Server
	> sudo tailscale status
Verified Server node was online
Identified Client node as offline
Confirmed Tailscale GUI unresponsive on Client

Resolution
Executed on Client:
	> tailscale logout
	> tailscale up
This forced:
	session reset
	device reauthentication
	WireGuard reinitialization
	node reconnection to Tailscale network

Result
Client node returned online
SSH connectivity restored successfully

Preventive Notes
Potential triggers:
	Windows sleep/hibernate
	abrupt network switching
	stale Tailscale session
	crashed/unresponsive Tailscale GUI/service
Recommended checks on Client:
	> sc query Tailscale
	> tailscale status
	> tailscale ping <peer>

