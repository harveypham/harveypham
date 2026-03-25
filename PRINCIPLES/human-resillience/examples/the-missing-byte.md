# The Missing Byte

<p align="right">Refined with assistance of AI tools</p>

We built a software that managed the security subsystem of a Self‑Encrypting Drive. Because we issued raw ATA commands directly, we had to follow best practices ourselves — including the rule that all transfers should be in multiples of the page size for optimal performance.

One of our drive vendors partnered closely with us during development. Early on, we discovered their firmware had a peculiar bug: whenever we requested N bytes, the drive returned N–1. An engineer came up with a clever workaround — simply request one extra byte and ignore the last byte. Since we never hit the end of the addressable space, it seemed harmless and worked reliably.

What we didn’t realize was that this workaround quietly violated the page‑alignment rule. The performance impact was subtle enough that we never noticed it in day‑to‑day use. Not until years later.

The problem finally surfaced when another vendor introduced a hybrid drive — a rotational disk with a small solid‑state cache. Under ideal access patterns, it could achieve about 90% of SSD performance. But in our tests, we never saw the gains they advertised. After long debugging sessions on both sides, including inspecting ATA packets with a SATA analyzer, we discovered the root cause: our extra‑byte workaround broke their caching logic. They were surprised by our access pattern and pointed out that we weren’t following established best practices.

Although the original vendor had already fixed the firmware in their next‑generation drives, we couldn’t simply remove the workaround — doing so would break compatibility with the first‑generation units already deployed in the field. So we created a second workaround, which we jokingly called “punish the offender.” We first issued a request with the correct size. If the drive returned one byte short, we issued a second request with the extra byte. Drives that followed the spec completed on the first call; the buggy drive required two.

The entire situation could have been avoided if we had escalated the bug instead of engineering around it.

The lesson is not that humans adapt — that part is expected. Human adaptation is the baseline: the natural, immediate response when a system misbehaves. The caution is that we relied on adaptation instead of requesting a system fix. In this case, the baseline was easily surpassed. The vendor could have corrected the bug quickly, improved their firmware, and avoided reputational risk. Our adaptation masked the flaw until it became structural debt.

Human resilience kept the system running, but the system should have been corrected.
