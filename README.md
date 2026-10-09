# Pulse Investor Pipeline

Version 0.1.1 packages three skills and an anonymous investor matching connector in one plugin. Find investors for your raise, draft outreach using your own company facts, and optionally connect your Pulse account to share a tracked deck link. Matching requires no Pulse account. Outreach drafts are yours to send.

| Skill | What it does | Connection |
|---|---|---|
| Find my investors | Confirm a raise profile and return a sourced shortlist. | Bundled anonymous matching connector |
| Draft my outreach | Draft firm-level emails, application answers and introduction blurbs. | No connection required for founder-provided facts |
| Share my deck | Create a tracked deck link and read selected-deck analytics. | Optional authenticated Pulse connection |

## Install in Claude web or desktop

Plugins are available on Claude Pro, Max, Team and Enterprise. Organization policy may control installation and connectors. See [Claude plugin availability](https://support.claude.com/en/articles/13837440-use-plugins-in-claude).

1. Download `pulse-skills-plugin.zip` from the distributor who supplied this package.
2. Open **Customize > Plugins > Add > Upload plugin** and select the ZIP. This installs all three skills together.
3. Open the plugin’s **Connectors** tab, add or activate **pulse-investor-match**, and choose **No sign in** if prompted. Its URL is `https://www.usepulse.co/investors/mcp`.
4. Start a chat, enable the matching connector through **+ > Connectors** when needed, and ask: “Who should I pitch for our seed round?”

The [official plugin guide](https://claude.com/docs/plugins/build) describes ZIP upload and connector activation. The connector still needs activation after plugin installation. This package has passed structural checks; fresh Claude installation and authenticated sharing remain unverified. No directory listing is claimed.

## Optional: share a deck

When you want a tracked deck link, add a separate custom connector under **Customize > Connectors** using `https://api.usepulse.co/mcp`, then complete Pulse sign-in and enable it in the chat. This authenticated connector is deliberately optional and is not preconfigured in the plugin. Ask “Share my deck” after connecting. The skill uses your authorized account and requests approval for any sensitive action requiring confirmation.

If Claude cannot upload file bytes, upload your PDF in [Pulse Drive](https://app.usepulse.co/drive), wait for processing, then tell Claude the document name. Creating a link does not send it to investors. Analytics must come from the tools; the skill cannot infer investor intent from page views.

## Free matching with a custom connector

Claude Free supports one custom connector. You can use matching without installing any skills: add `https://www.usepulse.co/investors/mcp` under **Customize > Connectors**, select **No sign in**, and enable it in your chat. Ask for investors matching your location, stage, sector and round size. This provides the matching tools; the plugin’s guided workflows require a plugin-capable plan. See [Claude remote connector setup](https://support.claude.com/en/articles/11175166-get-started-with-custom-connectors-using-remote-mcp). Claude plan limits and Pulse account entitlements are separate.

## Other clients

Claude Code can load an unpacked plugin with `claude --plugin-dir /absolute/path/to/plugin`; check `/mcp` for matching availability. For optional sharing, run `claude mcp add --transport http pulse https://api.usepulse.co/mcp` and use `/mcp` to sign in. Individual skill ZIPs are also available for Agent Skills clients; configure their MCP connections separately. Portable and Gemini manifests include anonymous matching only. Gemini runtime installation and account-specific availability in other assistants remain unverified.

## Data and support

Matching sends a confirmed short raise profile to Pulse, not your deck bytes. Do not include confidential deck contents in a matching request. Matching results should cite their sources; the [matching rubric](skills/find-my-investors/references/matching-rubric.md) explains assessment. Outreach uses supplied facts and never sends messages. Optional deck upload and sharing use your authorized Pulse account and the selected document’s actual tools and settings.

[Pulse website and support](https://www.usepulse.co) · [Privacy policy](https://www.usepulse.co/privacy-policy) · [Terms of service](https://www.usepulse.co/terms-of-service)

The package includes [MIT license terms](LICENSE). Installation does not establish an endorsement or a public directory listing.
