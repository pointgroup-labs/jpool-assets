# Validator Accelerator data

`program.json` is the public data source for JPool's temporary stake delegation program for newly launched Solana validators. The program appears as **Validator Bootstrap** on the JPool website: one validator can receive JPool stake while building an operating history, and approved validators wait in a queue for the next slot.

Public URL: `https://assets.jpool.one/validator-accelerator/program.json`

The file records the current program state, not the eligibility rules. Its free-text values, such as `history[].details`, are written in English and displayed as supplied. Fixed interface labels are maintained separately by the website.

## JSON structure

```ts
type Program = {
  enabled: boolean
  applicationFormUrl: string
  duration: number
  active: ActiveDelegation | null
  queue: QueueEntry[]
  history: HistoryEntry[]
}

type ActiveDelegation = {
  voteAccount: string
  amount: number
  startedAt: string
  endsAt: string
}

type QueueEntry = {
  voteAccount: string
  approvedAt: string
}

type HistoryReason =
  | 'external_delegation'
  | 'expired'
  | 'violation'
  | 'terminated'

type HistoryEntry = {
  voteAccount: string
  amount: number
  startedAt: string
  endedAt: string
  reason: HistoryReason
  details?: string
}
```

| Field | Meaning |
| --- | --- |
| `enabled` | Whether the program is currently available for applications. When `false`, the website shows it as temporarily unavailable and disables the application action. |
| `applicationFormUrl` | URL of the external application form. An empty string means no form link is available, so the application action is disabled. |
| `duration` | Maximum planned support period for one validator, in whole days. |
| `active` | The validator currently receiving JPool delegation, or `null` when the slot is empty. |
| `queue` | Approved validators waiting for a slot, in queue order. The first item is next. Use `[]` when empty. |
| `history` | Completed delegations. The website displays them by `endedAt`, newest first. Use `[]` when empty. |

### Entry fields

| Field | Used in | Meaning |
| --- | --- | --- |
| `voteAccount` | `active`, `queue[]`, `history[]` | Full Solana validator vote-account address, as a base58 string. |
| `amount` | `active`, `history[]` | Amount delegated by JPool, in **SOL**, not lamports. |
| `startedAt` | `active`, `history[]` | Date the delegation began. |
| `endsAt` | `active` | Planned end date of the current delegation. |
| `approvedAt` | `queue[]` | Date the validator was approved for the queue. |
| `endedAt` | `history[]` | Actual end date of a completed delegation. |
| `reason` | `history[]` | One of the outcome codes below. |
| `details` | `history[]` | Optional English explanation shown with the outcome. Omit the field when no explanation is needed. |

All dates are strings in `YYYY-MM-DD` format and represent UTC calendar dates.

## History reasons

| `reason` | Website label | When to use it |
| --- | --- | --- |
| `external_delegation` | Graduated | JPool support ended because the validator received sufficient stake from outside JPool. |
| `expired` | Completed | The planned support period finished. |
| `violation` | Violation | The validator breached a delegation rule. Explain the breach in `details`. |
| `terminated` | Terminated | Support ended early for another reason, such as prolonged downtime. Explain the reason in `details`. |

The website recognizes these four codes. A new `reason` requires a corresponding outcome label and styling in the website; otherwise the raw code is shown.

## Updating the file

- Keep `program.json` valid JSON and preserve the field names and types above.
- Use `null` for `active` when no validator is receiving support. Keep `queue` and `history` as arrays.
- Put only English text in `details`; it is displayed directly from this file.
- Keep the queue in the intended order and enter amounts in SOL.
