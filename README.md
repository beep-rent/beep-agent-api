# BEEP.rent — car rental in Poland

Search, price and book rental cars across Poland; the customer confirms on beep.rent.

This repository describes the public [Model Context Protocol](https://modelcontextprotocol.io)
server of BEEP.rent: tool schemas, examples and policies. The server itself is
hosted by BEEP; there is nothing to install. Registry name: `rent.beep/car-rental`.

## Connect

- **Any MCP client** (Streamable HTTP, no sign-in): `https://mcp.beep.rent/mcp`
- **Claude:** Settings → Connectors → Add custom connector → `https://mcp.beep.rent/mcp`
- **Claude Code:** `claude mcp add --transport http beep https://mcp.beep.rent/mcp`
- **ChatGPT:** add a custom MCP server with `https://mcp.beep.rent/chatgpt/mcp` (search and checkout; booking finishes on beep.rent)
- **Cursor, VS Code, Gemini CLI:** see [examples/](examples/)

Full guide: https://beep.rent/en-pl/ai-agents/ (Polish: https://beep.rent/pl-pl/dla-agentow-ai/).

## Tools

| Tool | Title | Kind |
|---|---|---|
| `search_offers` | Search rental offers | read-only |
| `get_offer` | Read an offer and its conditions | read-only |
| `get_city_prices` | Read indicative city prices | read-only |
| `find_pickup_points` | Find nearby pickup points | read-only |
| `resolve_booking_link` | Open a beep.rent offer link | read-only |
| `create_checkout` | Prepare a checkout | write |
| `create_reservation` | Book the reservation | destructive, needs the user's confirmation |
| `request_confirmation_code` | Send the confirmation code again | write |
| `confirm_reservation_code` | Confirm a reservation with the user's code | write |
| `get_reservation_status` | Read a reservation's status | read-only |
| `list_my_reservations` | List my reservations | read-only |
| `start_reservation_management` | Start managing reservations | write |
| `verify_reservation_management` | Verify the management code | write |
| `cancel_reservation` | Cancel a reservation | destructive, needs the user's confirmation |
| `quote_extension` | Price a longer rental | read-only |
| `request_extension` | Request a rental extension | destructive, needs the user's confirmation |

The ChatGPT profile exposes `search_offers`, `get_offer`, `get_city_prices`, `find_pickup_points`, `create_checkout`, `get_reservation_status`, `list_my_reservations`, `cancel_reservation`, `quote_extension`, `request_extension`. Full schemas, as `tools/list`
returns them: [tools/](tools/).

## How booking works

1. `search_offers` returns offers with the full price, the deposit and the policies.
2. `get_offer` reprices an offer with options; `create_checkout` freezes it and returns a beep.rent link.
3. `create_reservation` books with the driver's details after the user accepted the terms.
4. The user confirms with the 6-digit code they received (`confirm_reservation_code`) or pays on beep.rent.
   Nothing is confirmed before that; agents never take card details.

Cancelling at least 24 hours before pickup refunds everything paid. Prices are
the same as on beep.rent, with no surcharge for booking through an assistant.

## Terms, privacy, contact

- [Terms of the AI agent channel](https://beep.rent/en-pl/ai-agent-channel-terms/) · [Privacy notice](https://beep.rent/en-pl/ai-agent-channel-privacy/)
- Support: agents@beep.rent · Security: see [SECURITY.md](SECURITY.md)
- This repository: MIT ([LICENSE](LICENSE)). BEEP.rent and its data are not covered by it.
