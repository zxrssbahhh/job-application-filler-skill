# Browser-extension and proxy troubleshooting

Use this guide only when the selected browser cannot be controlled or file upload repeatedly fails. The same symptom can have different causes; do not assume Clash or another proxy is responsible.

## Diagnose in order

1. Confirm the browser family the user selected. Do not substitute Chrome for Edge or Edge for Chrome merely because a component uses Chromium terminology.
2. Check that the ChatGPT/Codex browser-control extension is installed for that browser and enabled in the app's Computer use settings.
3. Check that the extension is allowed to run on the current `http` or `https` site. A working ChatGPT sidebar does not by itself prove that the browser-control channel is connected.
4. Test control on a simple ordinary webpage. Do not use `about:blank`, browser settings, extension stores, or security pages as the only test.
5. Restart the selected browser and Codex, then retry discovery once.
6. Inspect whether a proxy, VPN, TUN mode, firewall, or split-routing rule treats Codex traffic and local extension traffic differently.

## Clash and similar proxies

Potential conflicts include Codex requiring the proxy while the browser extension store or target site fails through it, system proxying capturing loopback traffic, TUN and system-proxy modes routing differently, missing local/LAN bypass rules, and the browser and Codex being started under different proxy states.

Do not prescribe one universal fix. Propose one reversible test at a time, such as temporarily toggling system proxy, testing TUN versus system-proxy mode, or adding a local-address direct/bypass rule. Preserve the original configuration so it can be restored.

Before changing any proxy setting, browser site permission, extension permission, firewall rule, or system setting, explain the exact change and ask for confirmation at action time. Never weaken HTTPS, browser security, antivirus, or OS protections to make automation work.

If an extension store is reachable only with the proxy off but Codex requires the proxy on, a possible diagnostic sequence is:

1. record the current proxy state;
2. temporarily disable the relevant proxy mode only long enough to install or update the extension;
3. restore the proxy;
4. test whether local/loopback traffic needs a direct rule;
5. restart the browser and Codex;
6. verify control on an ordinary webpage.

Treat this as a hypothesis, not a guaranteed solution.

## Upload failures

- Use the browser's documented file-chooser event before clicking the visible upload control.
- Prefer a visible upload button when clicking a hidden file input does not emit a chooser event.
- Verify the filename rendered by the page.
- After two failed, freshly observed attempts, stop and give the user an exact manual upload instruction rather than opening duplicate tabs or repeatedly triggering native dialogs.
