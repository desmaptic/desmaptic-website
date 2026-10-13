<!-- updated: 2026-10-12 -->
# Away from home

At home, Desmaptic on your iPhone or iPad finds your Mac on the local network by itself. Away from home, it can reach the same Mac through your own VPN. Tailscale is the easiest way. The connection still goes straight from the phone to the Mac, encrypted, with no Desmaptic server in between.

Three rules keep it safe:

- **It is off until you turn it on** on the Mac.
- **Only a phone you already paired at home can connect.** A new device pairs only on your local network, with your click on the Mac and the code.
- **Only with App Lock on.** Away from home, Desmaptic asks for Face ID or your passcode each time you open it.

## What you need

- Your Mac at home, awake, with Desmaptic Agent running.
- Your iPhone or iPad paired with that Mac ([Get started › Pair your iPhone or iPad](?p=start#pair-your-iphone-or-ipad)).
- A passcode on the phone: App Lock uses it, or Face ID.
- Tailscale on the Mac and on the phone, signed in to the same account. Another VPN works too: [Other VPNs](#other-vpns).

## Set it up

> [!STEP] 1 · Install Tailscale on the Mac and on the phone
>
> Install Tailscale from [tailscale.com/download](https://tailscale.com/download) on the Mac and from the App Store on the iPhone or iPad, and sign in to the same account on both.
>
> Done when Tailscale on each device shows it connected and lists the other one.

> [!STEP] 2 · Allow connections over VPN on the Mac
>
> Click the Desmaptic Agent icon (a rounded square, the key from the Desmaptic app icon) in the menu bar, choose **Show Agent Window** (⌘O), then **Security** in the sidebar. Under **Right now**, turn on **Allow Connections over VPN**. Its caption reads: "Devices connect only on your local network. Turn this on to let paired devices also connect through a VPN, such as Tailscale. New devices pair only on your local network, and the internet is never let in."
>
> Done when the row **Shared over VPN** appears under it with **Tailscale** and this Mac's addresses: "This Mac tells your paired devices its VPN addresses: …". If it reads **No VPN**, Tailscale isn't connected on this Mac.

> [!STEP] 3 · Turn on App Lock on the phone
>
> In Desmaptic on the phone, open **Settings › Security** and turn on **App Lock**. Under it, the phone says how it unlocks, such as "Unlocks with Face ID". The footer adds: "App Lock is also needed to connect to your Mac over VPN away from home."
>
> No passcode on the phone? The switch stays off: "Set a passcode in iOS Settings to use the app lock."

> [!STEP] 4 · Connect once at home
>
> Open Desmaptic on the phone while it is on your home network. The Mac hands it its Tailscale addresses, and the phone says **Ready for away from home**: "“MacBook Pro” is reachable via Tailscale. Away from home, just open the app."
>
> Done when, in the agent's **Security**, **Shared over VPN** names your phone: "“iPhone” received them on …".

> [!SEE]
> Away from home, turn on Tailscale on the phone and open Desmaptic. It asks you to unlock: "Away from home, Desmaptic asks you to unlock it each time you open it." After **Unlock**, your deck works as at home, and the connection badge at the top shows a small **VPN** tag after your Mac's name.
>
> Not seeing it? [If it doesn't connect](#if-it-doesnt-connect).

## Away from home, day to day

- **Unlock on every opening.** At home, App Lock asks only before someone edits the deck, opens the Gallery, pairs a new Mac or approves a change. Over VPN, it also asks each time you open the app, whatever **Ask Again After** says. Until you unlock, the deck is covered and taps reach nothing; **Disconnect** leaves the Mac.
- **The VPN tag** after your Mac's name in the connection badge means this connection goes through your VPN, not your home network.
- **Your Mac's addresses stay current.** If they change, for example after you reinstall Tailscale, the Mac sends the new ones to the connected phone. A phone that was away meanwhile gets them the next time it connects at home; until then **Shared over VPN** reads: "“iPhone” has older addresses from …. Connect it on your local network to update them."
- **Live widgets work as at home**, through the VPN. Over mobile data they may update a little later.
- **Pairing a new device works only at home**, on the Mac's own network.

### What Paired Macs says

Tap the connection badge at the top of the deck to open the list of your Macs. Under **Paired Macs**, each Mac has a line about away from home; **Details** opens what the Mac shared and when.

| Line | What to do |
|---|---|
| **Away from home: ready via Tailscale** | Nothing: away from home, turn on Tailscale on the phone and just open the app. |
| **Away from home: ready via VPN** or **ready via your address** | The same with your own VPN. |
| **Away from home: App Lock is off** | Turn on App Lock: in **Details**, **Turn On App Lock**, or in **Settings › Security**. |
| **Away from home: VPN is off on the Mac** | On the Mac, turn on **Allow Connections over VPN** in the **Security** section of Desmaptic Agent. |
| **Away from home: the Mac isn't in a VPN** | Install Tailscale on the Mac and on this device and sign in to the same account on both, or [add your own address](#your-own-address). |
| **Away from home: not set up** | Update Desmaptic Agent on the Mac and connect on your home network once, or [add your own address](#your-own-address). |

## Tailscale access rule

Desmaptic Agent lets in only devices you paired, so another device in your tailnet can knock but can't get in. You can also stop it from knocking at all: give Tailscale a rule that lets only your phone reach the Mac, and only on port 9847, the port Desmaptic uses.

Rules live in the Tailscale admin console, under **Access controls**. First look up the Tailscale addresses of your phone and your Mac: the admin console lists them under **Machines**, an IPv4 address that starts with `100.` and an IPv6 address that starts with `fd7a:`. Put them into this rule in place of the examples:

```json title="Tailscale policy file"
{
  "hosts": {
    "my-iphone":      "100.101.102.103",
    "my-iphone-ipv6": "fd7a:115c:a1e0::1a2b:3c4d",
    "my-mac":         "100.88.77.66",
    "my-mac-ipv6":    "fd7a:115c:a1e0::5e6f:7a8b"
  },
  "grants": [
    {
      "src": ["my-iphone", "my-iphone-ipv6"],
      "dst": ["my-mac", "my-mac-ipv6"],
      "ip":  ["tcp:9847"]
    }
  ],
  "tests": [
    { "src": "my-iphone", "accept": ["my-mac:9847"] }
  ]
}
```

`tests` makes Tailscale refuse to save a policy that would cut your phone off from the Mac.

> [!WARNING] The rule only narrows things if nothing wider is left
> Tailscale adds rules up: a connection is let through if any rule allows it. A new tailnet starts with one that lets every device reach every other on every port: a grant with `"src": ["*"]`, `"dst": ["*"]` and `"ip": ["*"]`, or in older files an `acls` entry with `"src": ["*"]` and `"dst": ["*:*"]`. While it stays, the rule above changes nothing. Replace it with rules for what you really use, such as file sharing or SSH between your own devices, or those stop working too.

## What not to do

- **Don't put Tailscale Serve or Funnel on port 9847.** The phone reaches the agent directly at the Mac's Tailscale address and needs neither. Through `tailscale serve`, a connection reaches the agent from the Mac itself, so the agent can't tell who is behind it. `tailscale funnel` goes further and opens the port to the whole internet.
- **Don't turn off "Allow incoming connections" in Tailscale on the Mac.** It is in the Tailscale menu bar app, under **Settings**, and is on by default. Off, it blocks every connection from your tailnet to the Mac, your phone's included. The CLI calls it `--shields-up`.
- **Don't forward port 9847 on your router.** Desmaptic Agent never lets in a connection from the internet, so a port forward only exposes the port.

## Other VPNs

| Your VPN | Works? | How |
|---|---|---|
| **Tailscale** | Yes | The steps above. |
| **ZeroTier, NetBird, WireGuard on the Mac** | Yes | The same steps with your VPN in place of Tailscale. The Mac shares its address in that VPN, and Paired Macs reads **ready via VPN**. If the phone still can't reach it, [add your own address](#your-own-address). |
| **WireGuard or another VPN on your home router** | Yes | The phone joins your home network, so [add your own address](#your-own-address): the Mac's address at home or its name. |
| **A privacy VPN, such as NordVPN, ExpressVPN, Surfshark or Proton VPN** | No | [Privacy VPNs](#privacy-vpns) |

### Your own address

Use it when the addresses the Mac shares itself don't work: a VPN into your home network, like WireGuard on your router, or your own DNS name for the Mac.

Tap the connection badge, then **Add Address Away from Home** under your Mac in **Paired Macs**. Enter a name or an IP address, optionally with a port, like `mac.example.com` or `10.0.0.5:9847`. **Check** tries it once and says what it found; **Save** keeps it. Later the line reads "Your address: …": tap it to change the address or **Remove Address**.

| Check says | What to do |
|---|---|
| "“MacBook Pro” answered at this address." | **Save** it. |
| "“MacBook Pro” answered at this address. VPN is off on this device, so it will be used once a VPN is on." | **Save** it: the phone uses it away from home, with your VPN on. |
| "No answer from this address. “MacBook Pro” may be asleep or turned off, or your VPN may not be connected." | Check that the Mac is awake and your VPN is connected. |
| "The connection was closed at this address. On “MacBook Pro”, turn on Allow Connections over VPN in Desmaptic Agent, …" | Turn on **Allow Connections over VPN**, or switch back to your account on the Mac. A port forward on your router is always refused: use an address in your VPN. |
| "Nothing answered at this address. Check the name or IP address and the port, and that your VPN is connected." | Check the address and the VPN. |
| "Something answered at this address, but it isn't Desmaptic Agent. Check the port." | Use port 9847, or leave the port out. |
| "A different Mac or user account answered at this address, not “MacBook Pro”. …" | Check the address, or switch back to your account on the Mac. |

The phone tries your address when the Mac isn't on the same network and a VPN is on, after the Mac's own addresses. The address must lead to the Mac through a VPN: Desmaptic Agent refuses connections from the internet. If it leads into your home network, through a VPN that ends on your router, the Mac sees the phone as a device at home. For Tailscale or another tunnel that ends on the Mac, **Allow Connections over VPN** has to be on.

### Privacy VPNs

NordVPN, ExpressVPN, Surfshark, Proton VPN and the like send your traffic to the internet through their servers. They don't connect your devices to each other: through them, the phone would reach your Mac only from the internet, and Desmaptic Agent never lets the internet in. Away from home, use Tailscale or a VPN into your home network instead.

At home, such a VPN on the phone can hide your Mac. If the phone stops finding it, allow local network access in that VPN's settings, or pause it. The same goes for a Tailscale exit node on the phone: turn on **Allow Local Network Access** in Tailscale's exit node settings.

## If you lose your phone

A lost phone is still guarded by its passcode and, away from home, by App Lock. Cut it off anyway:

1. **Remove it from Tailscale**, from any browser: in the Tailscale admin console, open **Machines**, then the phone's **⋯** menu, **Remove**. It loses its way into your tailnet at once. If your Tailscale sign-in is saved on that phone, also change your password or sign out its sessions with your sign-in provider, so it can't join again.
2. **Remove it on the Mac** as soon as you can get to it: in the agent window, open **Devices** and click **Remove** next to the phone. The Mac stops trusting it at once, over any network: to connect again, it would have to pair at home with your click on the Mac and a new code.
3. **Find My** on another device can mark the phone as lost or erase it.

A new phone, or the same one added to Tailscale again, gets new Tailscale addresses. If you use the [access rule](#tailscale-access-rule), put them in place of the old ones in `my-iphone` and `my-iphone-ipv6`, or the phone can't reach the Mac away from home. The rule's `tests` won't catch it: they check the addresses written in the rule.

Turning off **Allow Connections over VPN** in **Security** also works: it closes every connection that came over a VPN at once and lets none in until you turn it on again, for all your devices.

## If it doesn't connect

When the phone can't reach your Mac away from home, the list of your Macs (tap the connection badge) says why.

| You see | Why | Fix |
|---|---|---|
| "“MacBook Pro” isn't on this network, and VPN is off on this iPhone. Turn on your VPN to connect away from home." | Tailscale is off on the phone | Turn on Tailscale on the phone. Desmaptic then connects by itself. |
| "“MacBook Pro” isn't on this network. To use it over VPN away from home, turn on App Lock: …" | App Lock is off | **Turn On App Lock** under the notice. Without a passcode on the phone, the notice asks you to set one in iOS Settings first. |
| "Turn on App Lock to use your Mac away from home" over the deck | App Lock was turned off while connected over VPN | **Turn On App Lock**, or **Disconnect**. |
| "“MacBook Pro” closed the connection over VPN. On that Mac, turn on Allow Connections over VPN in Desmaptic Agent, …" | **Allow Connections over VPN** is off, or another user is using the Mac | Turn the switch on in the agent's **Security**, or switch back to your account on the Mac. |
| "Couldn't reach “MacBook Pro” over VPN. It may be asleep or turned off, Desmaptic Agent may not be running, or the Mac isn't connected to the VPN." | The Mac sleeps or is off, the agent isn't running, Tailscale on the Mac is disconnected, or your [access rule](#tailscale-access-rule) doesn't let this phone through | Keep the Mac awake: on a MacBook, **System Settings › Battery › Options › Prevent automatic sleeping on power adapter when the display is off**; on a desktop Mac, **System Settings › Energy › Prevent automatic sleeping when the display is off**. Check that Desmaptic Agent runs and Tailscale on the Mac is connected. After a new phone, put its addresses into the rule. |
| "Couldn't confirm this is “MacBook Pro” over VPN — most likely another user account is active on that Mac. …" | Another user is using the Mac, or Desmaptic Agent was reinstalled there | Switch back to your account on the Mac: the phone tries again later. After a reinstall, pair again at home. |
| **Shared over VPN** reads **No VPN** in the agent | The Mac isn't in a VPN | Connect Tailscale on the Mac. |
| At home, the phone can't find the Mac while Tailscale is on | An exit node or a privacy VPN on the phone hides the local network | [Privacy VPNs](#privacy-vpns) |

With your own address, **Check** says what is wrong: [Your own address](#your-own-address).

Anything else: [Troubleshooting](?p=troubleshooting).
