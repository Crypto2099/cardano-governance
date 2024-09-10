```yaml
ID: 15f82a365bdee483a4b03873a40d3829cc88c048ff3703e11bd01dd9e035c91600
Title: Name the next hard fork HOSKY Hard Fork
Date: Sep 3, 2024 12:45:50 PM
```

# Publication Rationale

> **Full Disclosure**: I, Adam Dean, was the sole creator and submitter of this
> governance action. So, my endorsement, support, or argument for or against
> this proposal should be taken with a healthy grain of salt.

In mid-June 2024, a Cardano SPO and "influencer" by the name
of [@StakeWithPride](https://x.com/StakeWithPride) made a post wondering how
hard forks after Chang would be named given that previously this had been
exclusively the realm of IOG as the developers and Genesis Key holders.

In [this tweet](https://x.com/StakeWithPride/status/1800618878243942620) PRIDE
indicated:

> Hard forks after Chang should probably be named by the community via a
> governance vote.

Charles Hoskinson, founder of Cardano, indicated
[his support in a reply](https://x.com/IOHK_Charles/status/1800894722527080700).

Having entered into the Age of Voltaire with the initial Chang Hard Fork and,
after several days, seeing that no governance actions had been proposed, I took
it upon myself to kickstart the conversation and the governance process by
creating a playful tongue-in-cheek proposal to name the next hard fork after
HOSKY.

The thought process here was that we needed a real-world, functional test of
both the newly created dReps and self-voters, SPOs, as well as the Interim
Constitutional Committee (ICC) and a way to ensure that main net tooling was
also functional and working as expected before a "real" proposal was submitted.

## Voter (SPO/dRep) Opinion

For me, personally, hopefully the network will never name something as important
and impactful as a hard fork after a community member or project that is so
obviously satire. Sorry, HOSKY, but you're just not serious enough to have a
hard fork named after you. Thus, were I a voting member of the community (SPO or
dRep) I would `Vote No` on this proposal.

> **Note**: As of the time of writing this rationale I have already retired
> years ago from being an SPO and I have no intent to start running a stake pool
> again in the future and I have no plans to become a public dRep and, in fact,
> will most likely delegate my vote to a dRep.

## Constitutional Committee (ICC) Opinion

There was an interesting yet unintentional issue when publishing this action
that warrants additional consideration and discussion.

Article 3, Section 6, Paragraph 1 of the
[Interim Cardano Constitution](https://constitution.gov.tools/en/interim-constitution#section-6)
states:
> Any governance action submitted to Ada holders for approval shall require a
> standardized and legible format including a URL and hash linked to documented
> off-chain content ... The rationale shall include, at a
> minimum, a title, abstract, reason for the proposal, and relevant supporting
> materials.

What is interesting about this section and this first published governance
action on-chain is that, presumably, the intent here was that the `hash` in
question would be a hash of the published files entire contents in order to
assure that the _contents_ of the file have not been changed or modified since
publication.

The expected standard adopted by most current governance tools is that the
hashing of the file should conform to the standard defined in
[CIP-100](https://github.com/cardano-foundation/CIPs/tree/master/CIP-0100#hashing-and-signatures).
This standard (currently) states that:

> ... the hash in the anchor should be the blake2b-256 hash of the raw bytes
> received from the wire

However, due to a shell variable naming issue, the hash of the canonized body of
the JSON-LD document was published to the chain instead of the hash of the file
contents itself.

**TL;DR: The hash in the Governance Action doesn't match _current_ standards for
metadata**

### What does that mean?

This is an interesting, albeit unintended, opportunity to discuss and consider
both the language of the Constitution itself and the possibility that standards
and algorithms expected for hashing may change over time. For example, the
CIP-100 Standard changed as recently
as [June 25th, 2024 [GH Commit 4640b74]](https://github.com/cardano-foundation/CIPs/commit/4640b74025c4d7f233c47ebc8319e634d2de39de).

### But, will it hash?!

> **TL;DR: YES!**

The anchor URI to the metadata was published as:
`ipfs://QmWjcHsrq9kKHZZ7aPPFjqN6wLuxH9d8bcqssmrE7H4cvb`

Publicly accessible file
URL: [https://ipfs.io/ipfs/QmWjcHsrq9kKHZZ7aPPFjqN6wLuxH9d8bcqssmrE7H4cvb](https://ipfs.io/ipfs/QmWjcHsrq9kKHZZ7aPPFjqN6wLuxH9d8bcqssmrE7H4cvb)

The posted metadata hash was:
`2f98f57c4149fdfed2b73cbd821226fe417ef5ed49d8f836a37b31edf14dea47`

This hash **does not** conform to the expected `blake2b-256` hash of the file
contents which should be:
`a5f2785526d91ec3f7cdd642b679ee668f4483d0f1af045b3bf2750f1fcc3dcb`

Code to replicate and confirm over-the-wire, CIP-100 compatible hash:

```shell
curl -s https://ipfs.io/ipfs/QmWjcHsrq9kKHZZ7aPPFjqN6wLuxH9d8bcqssmrE7H4cvb -o gov.action.1.json
b2sum -l 256 gov.action.1.json
```

**Expected Output**

```shell
dev@null:~$ b2sum -l 256 gov.action.1.json
a5f2785526d91ec3f7cdd642b679ee668f4483d0f1af045b3bf2750f1fcc3dcb  gov.action.1.json
```

So, where
does `2f98f57c4149fdfed2b73cbd821226fe417ef5ed49d8f836a37b31edf14dea47`
come from? Simple! This is specifically the hash of the canonized `body` of the
JSON-LD document that is used by authors as a signing and verification point to
confirm that an author's signature belongs to this body text! This also means
that we can use the same process we would normally to validate author signatures
to prove that this hash does, in fact, match to this document.

> For the ease and simplicity of creating and validating the `body` hash of a
> JSON-LD file I always recommend the use of the amazing
> [Cardano Signer](https://github.com/gitmachtl/cardano-signer/releases)
> library from the one and
> only [Martin Lang [ATADA]](https://github.com/gitmachtl).

Steps to Validate the Body:

1. Download and unpack the latest release version of Cardano Signer for your
   platform from
   [https://github.com/gitmachtl/cardano-signer/releases](https://github.com/gitmachtl/cardano-signer/releases)
2. Run the `canonize` command to generate the `blake2b-256` hash of the
   canonized body of the JSON-LD document:

```shell
dev@null:~$ ./cardano-signer canonize --cip100 --data-file gov.action.1.json
2f98f57c4149fdfed2b73cbd821226fe417ef5ed49d8f836a37b31edf14dea47
```

### Is it Constitutional?

Here we can see that the published hash does, indeed, match the `body` content
of the file URI and thus, could be considered a valid hash for the file. Note
that this hash would need to be calculated by any explorer that was validating
author signatures since co-authors of a proposal must sign this hash with their
private keys and then add their signature to the JSON-LD body of the file.

An argument could be made that any author signatures supplied along with this
JSON would be invalid (and potentially unconstitutional) because the contents of
the outer file may have been changed after publication. However, in this case
no authors were submitted as part of the governance action (psuedo anonymity)
and thus we can assure that the file contents were not changed or tampered
between authorship, publication, and retrieval from the blockchain.

### The Metadata

For your viewing pleasure, here is the unmodified contents of the file:

```json
{
  "@context": {
    "CIP100": "https://github.com/cardano-foundation/CIPs/blob/master/CIP-0100/README.md#",
    "CIP108": "https://github.com/cardano-foundation/CIPs/blob/master/CIP-0108/README.md#",
    "hashAlgorithm": "CIP100:hashAlgorithm",
    "body": {
      "@id": "CIP108:body",
      "@context": {
        "references": {
          "@id": "CIP108:references",
          "@container": "@set",
          "@context": {
            "GovernanceMetadata": "CIP100:GovernanceMetadataReference",
            "Other": "CIP100:OtherReference",
            "label": "CIP100:reference-label",
            "uri": "CIP100:reference-uri",
            "referenceHash": {
              "@id": "CIP108:referenceHash",
              "@context": {
                "hashDigest": "CIP108:hashDigest",
                "hashAlgorithm": "CIP100:hashAlgorithm"
              }
            }
          }
        },
        "title": "CIP108:title",
        "abstract": "CIP108:abstract",
        "motivation": "CIP108:motivation",
        "rationale": "CIP108:rationale"
      }
    },
    "authors": {
      "@id": "CIP100:authors",
      "@container": "@set",
      "@context": {
        "name": "http://xmlns.com/foaf/0.1/name",
        "witness": {
          "@id": "CIP100:witness",
          "@context": {
            "witnessAlgorithm": "CIP100:witnessAlgorithm",
            "publicKey": "CIP100:publicKey",
            "signature": "CIP100:signature"
          }
        }
      }
    }
  },
  "authors": [],
  "hashAlgorithm": "blake2b-256",
  "body": {
    "abstract": "The Chang+1 hard fork should be called the HOSKY Hard Fork",
    "motivation": "I propose to name the next Cardano hard fork 'HOSKY' after Cardano's mutt and the most widely recognized and held asset in the ecosystem.",
    "rationale": "Saying Chang-1, Chang-2, Chang+1 is confusing for users and media. Call the next hard fork HOSKY Hard Fork for the sake of clarity.",
    "references": [],
    "title": "Name the next hard fork HOSKY Hard Fork"
  }
}
```


