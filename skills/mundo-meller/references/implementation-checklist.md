# Mundo Meller — implementation checklist

## Identity and safety

- [ ] Store/market confirmed.
- [ ] Duplicate theme name and ID confirmed.
- [ ] Published theme identified and excluded from writes.
- [ ] Snapshot or rollback method recorded.
- [ ] Exact write batch authorized.

## Visual contract

- [ ] Desktop entry state approved.
- [ ] Mobile entry state approved.
- [ ] Filter, family, selection, quick-view, cart feedback, loading, empty, error, and fallback states approved.
- [ ] Typography, spacing, motion, imagery, and merchandising hierarchy documented.

## Data contract

- [ ] Product subset defined.
- [ ] Product type/use mapping defined.
- [ ] Shape mapping defined.
- [ ] Family/model mapping defined.
- [ ] Showroom eligibility and priority defined.
- [ ] Variant/color mapping verified.
- [ ] Price, availability, URLs, and media sourced from Shopify.
- [ ] Missing/invalid data behavior defined.

## Rendering and assets

- [ ] Tier A, B, or C selected with rationale.
- [ ] Initial LCP asset identified.
- [ ] Responsive images and dimensions configured.
- [ ] 3D/360 assets validated where applicable.
- [ ] Geometry and texture compression considered.
- [ ] Low-power/no-WebGL fallback complete.
- [ ] Reduced-motion behavior complete.

## Commerce

- [ ] Product title and price accurate.
- [ ] Variant selection accurate.
- [ ] Unavailable variants blocked.
- [ ] Add to cart integrates with active theme.
- [ ] Cart drawer/page feedback verified.
- [ ] Product-detail links verified.
- [ ] Dynamic checkout impact assessed.
- [ ] Wishlist/VTO/reviews integrations verified if included.

## Responsive and accessibility

- [ ] Mobile is touch-first rather than scaled desktop.
- [ ] No essential hover-only behavior.
- [ ] Keyboard navigation works.
- [ ] Focus is visible and logical.
- [ ] Controls have accessible names and states.
- [ ] Dialog/panel focus management works.
- [ ] Contrast and text scaling verified.
- [ ] Motion preferences respected.

## Performance

- [ ] LCP target <= 2.5 s.
- [ ] INP target <= 200 ms.
- [ ] CLS target <= 0.1.
- [ ] Critical route does not preload noncritical 3D/360 assets.
- [ ] JavaScript split by feature.
- [ ] Offscreen animation and rendering pause.
- [ ] Canvas DPR capped where applicable.
- [ ] Assets/listeners disposed on section unload.
- [ ] Before/after observations recorded.

## Shopify theme behavior

- [ ] Section settings valid and editor-friendly.
- [ ] CSS and JavaScript scoped by section instance.
- [ ] Design-mode section reload works.
- [ ] Customer-facing text uses locales.
- [ ] No fragile public-CDN dependency introduced.
- [ ] No live commerce value hardcoded.

## Analytics and errors

- [ ] Event names follow the skill event contract.
- [ ] Events fire once per intended action.
- [ ] No personal information included.
- [ ] Fallback and error events are measurable.
- [ ] Console and network errors resolved or disclosed.

## Release evidence

- [ ] Desktop screenshots compared with contract.
- [ ] Mobile screenshots compared with contract.
- [ ] Functional matrix completed.
- [ ] Accessibility checks completed.
- [ ] Performance observations completed.
- [ ] Published theme remains untouched.
- [ ] Remaining deviations and risks disclosed.
- [ ] Maintenance and rollback notes delivered.