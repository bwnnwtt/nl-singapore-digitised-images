# National Library Singapore digitised image assets

This repository contains a truncated copy of records extracted from the [National Library dataset on data.gov.sg](https://data.gov.sg/datasets?query=national+library&resultId=d_d269162117ff494b2832723e18b10e88), together with corresponding JPEG images retrieved from National Library Board (NLB) Singapore.

Dataset identifier: `d_d269162117ff494b2832723e18b10e88`.

The local source extract contained 5,000 records. Seven images could not be retrieved; their records were removed from the published JSON array. This snapshot contains **4,993 records and 4,993 matching JPEG images**, prepared on 12 November 2026. It is not the complete source dataset or a live mirror.

## Files

```text
.
├── README.md
├── .nojekyll
├── .gitattributes
├── index.html
├── nlb-first-5000-records-removed-missing-images.json
├── image-validation-report.json
└── images/
    └── <identifier_uuid>.jpg
```

- The records file is a UTF-8 JSON array. Original record fields and values are preserved; only the seven unavailable-image records were removed.
- `identifier_uuid` is the exact filename stem of the corresponding image. Preserve its case and any prefix, including `B-`.
- `identifier_uri` links to the original NLB image detail page. It is not the local image URL.
- `image-validation-report.json` describes validation against the original 5,000-record extract: `expected` is 5,000, `valid_jpegs` is 4,993, `missing` lists the seven excluded identifiers, and `invalid` is empty. Its original counts are intentionally retained.
- Images total 253,019,455 bytes (approximately 241.3 MiB). All 4,993 files were fully decoded and validated as JPEGs.

## Upload to GitHub

Copy the contents of this folder, including hidden files, into the root of your repository checkout. Commit and push using Git or GitHub Desktop. With almost 5,000 assets, Git is more practical than browser uploads, which allow up to 100 files at a time. Each file here is below GitHub's ordinary Git file-size limit; Git LFS is not needed.

For a newly created, empty GitHub repository, run these commands from this folder after replacing `OWNER` and `REPOSITORY`:

```sh
git init -b main
git add .
git commit -m "Add NLB image records and static assets"
git remote add origin https://github.com/OWNER/REPOSITORY.git
git push -u origin main
```

See [GitHub's file upload documentation](https://docs.github.com/en/repositories/working-with-files/managing-files/adding-a-file-to-a-repository).

## Serve with GitHub Pages

In repository **Settings → Pages**, choose **Deploy from a branch**, select `main` and `/(root)`, then save. Wait for the deployment to finish. The `.nojekyll` file allows direct static publishing without Jekyll processing. See [GitHub's publishing-source documentation](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site).

For a standard project repository, use these URL patterns, replacing `OWNER`, `REPOSITORY`, and the UUID:

```text
https://OWNER.github.io/REPOSITORY/nlb-first-5000-records-removed-missing-images.json
https://OWNER.github.io/REPOSITORY/image-validation-report.json
https://OWNER.github.io/REPOSITORY/images/<identifier_uuid>.jpg
```

No build step, API server, or database is required. Use the actual Pages URL shown by GitHub if you configure a custom domain or an account-level Pages repository.

Example JavaScript client:

```js
const base = 'https://OWNER.github.io/REPOSITORY/';
const response = await fetch(new URL('nlb-first-5000-records-removed-missing-images.json', base));
if (!response.ok) throw new Error(`Records request failed: ${response.status}`);
const records = await response.json();
const assets = records.map(record => ({
  ...record,
  image_url: new URL(`images/${encodeURIComponent(record.identifier_uuid)}.jpg`, base).href,
}));
```

For direct file retrieval without Pages, use a raw GitHub URL:

```text
https://raw.githubusercontent.com/OWNER/REPOSITORY/main/images/<identifier_uuid>.jpg
```

Replace `main` with a commit SHA when a consumer needs an immutable snapshot. GitHub's `/blob/` URLs are file-viewing pages, not direct image URLs.

## Provenance and rights

Metadata comes from the linked data.gov.sg dataset; images come from NLB's image service, matched by `identifier_uuid`. Original source links, descriptions, attribution, `rights`, and `access_rights` remain in each record.

These records include copyright and access restrictions, including notices limiting use to private research and study and requiring copyright-holder permission for copying or modification. This repository does not grant a new licence or establish permission to redistribute the images. Consult the individual records and source terms, and obtain any required permission before public hosting or reuse. No blanket open-source licence is applied to these third-party assets.

## Missing images

Seven image requests repeatedly returned `Parameter is not valid.` from NLB. Their identifiers are listed in `image-validation-report.json`; no placeholder files or substitute images are included. Consumers should use the filtered records file to enumerate available images.
