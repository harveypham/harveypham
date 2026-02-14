# The Modified Time Complexity

<p align="right">Refined with assistance of AI tools</p>

We built a system that connects to a customer’s cloud storage and syncs their invoices into our extraction pipeline. During design, a senior operator raised a real scenario: “Users receive invoices with the same filename every month.” This is common — many vendors send monthly invoices named identically.

The question became: What happens if a user uploads the new invoice and overwrites the previous one?  
To handle this, we added logic to detect file modifications and treat them as new invoices.

That decision introduced a surprising amount of complexity. Time‑zone conversions created subtle bugs. Metadata differences produced duplicate entries. Even after fixes, we discovered that a file’s “modified date” could change simply because a user viewed or interacted with the document — not because it was a new invoice. The system became fragile and harder to reason about.

Once we observed real customer behavior, the truth became obvious: customers weren’t uploading files with the same name at all. They had already adapted to the vendor’s naming limitation by renaming the invoice before uploading it to cloud storage.

In other words, the “corner case” we engineered around was already solved — by the humans.

This entire chain of complexity could have been avoided if someone had asked the key question:
**What would users do if this feature didn’t exist?**
A simpler solution — asking users to rename the file — was not only viable, but already happening in practice.