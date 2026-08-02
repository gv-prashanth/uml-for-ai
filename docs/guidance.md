# UML Diagram Reference

This file is the source of truth for UML notation, diagram organization,
symbol geometry, connector routing, and spacing in diagrams created from this
repository.

**Read this file before creating or modifying a diagram.** Explicit instructions
in the current user request take precedence; otherwise, apply these rules
without asking for confirmation.

## Measurement conventions

- Aspect ratios are written as **width:height**.
- Gaps are measured **edge to edge**, not center to center.
- Coordinates and dimensions are draw.io canvas pixels.
- Preserve these values in the editable .drawio source. A visually similar PNG
  is not sufficient.

## Aspect-ratio requirements

### General rule

Every non-container UML symbol must preserve its aspect ratio.

In draw.io XML, add:

    aspect=fixed;

to the style of each non-container symbol.

This applies to:

- UML actors;
- use-case ellipses;
- other non-rectangular semantic symbols introduced in future diagrams.

Do **not** apply a fixed aspect ratio to:

- system boundaries;
- rectangles and rounded rectangles;
- class, component, node, or other rectangular container boxes;
- notes and callout boxes;
- backgrounds, text labels, and legend containers;
- connectors.

### UML actors

The canonical actor geometry is:

| Property | Requirement |
| --- | --- |
| Width | 95 |
| Height | 190 |
| Ratio | 1:2 |
| Resize behavior | aspect=fixed |
| Stroke width | 2 |

Canonical style fragment:

~~~xml
style="shape=umlActor;aspect=fixed;verticalLabelPosition=bottom;verticalAlign=top;html=1;outlineConnect=0;strokeWidth=2;"
~~~

Canonical geometry fragment:

~~~xml
<mxGeometry width="95" height="190" x="..." y="..." as="geometry"/>
~~~

Actor names may use a separate wrapped text cell below the symbol. Do not widen
or flatten the actor to make a long label fit.

### Use-case ellipses

The canonical use-case geometry is:

| Property | Requirement |
| --- | --- |
| Width | 240 |
| Height | 160 |
| Ratio | 3:2 |
| Resize behavior | aspect=fixed |

Canonical style fragment:

~~~xml
style="ellipse;whiteSpace=wrap;html=1;aspect=fixed;..."
~~~

Canonical geometry fragment:

~~~xml
<mxGeometry width="240" height="160" x="..." y="..." as="geometry"/>
~~~

The aspect=fixed style preserves the ratio already present; it does not convert
an incorrect ratio. For example, 300 × 105 remains incorrect even when
aspect=fixed is set.

If a use-case label needs more room, scale both dimensions proportionally:

| Allowed example | Ratio |
| --- | --- |
| 240 × 160 | 3:2 |
| 300 × 200 | 3:2 |
| 360 × 240 | 3:2 |

Never change only the width or only the height.

## Use-case diagram organization

Unless the current user request explicitly overrides them, apply the following
layout rules to every new or reorganized use-case diagram.

### Actor placement

- Place all external or initiating actors on the far left, outside the system
  or subject boundaries.
- Place all internal, supporting, platform, runtime, and other downstream
  actors on the far right, outside the system or subject boundaries.
- Do not mix external and downstream actors on the same side.

### Actor selection and hierarchy

- Introduce a separate actor only when it has a meaningfully different
  responsibility.
- Reuse one actor across related workflows when the same operational role or
  external system performs them. Do not create separate API-client,
  administrator, or product-specific actor aliases without distinct behavior.
- Prefer the accountable role or system name over the invocation mechanism.
  A non-human supporting system may be an actor without an additional
  `«external system»` label.
- In an actor-generalization tree, place the more general actor above its
  specializations. Align sibling specializations on one horizontal row, space
  them evenly, and center the parent over the sibling group.
- Associate generic behavior with the general actor and topology-specific
  behavior with the appropriate specialization.

Generalization still means **is a kind of**. The hierarchy layout above does
not permit a hosting or containment relationship to be drawn as
generalization.

### Two vertical use-case columns

Organize the use cases into two vertical columns:

| Column | Required content |
| --- | --- |
| First / left column | External-facing use cases initiated or observed by external actors |
| Second / right column | Internal or downstream use cases performed against supporting systems and runtimes |

Do **not** draw a vertical separation line between the two use-case columns.
Use column headers, alignment, and whitespace to distinguish the external-facing
and downstream lanes. A divider adds a non-UML visual element and can be
mistaken for a relationship or system boundary.

### Straight connector requirement

All actor associations and use-case relationships in a use-case diagram must
use a single straight line rather than an orthogonal, elbowed, or stepped
route.

- Do not use `orthogonalEdgeStyle` for use-case diagram connections.
- Do not add intermediate waypoints merely to route around another element.
- If a straight connection would cross a symbol or label, move the symbols,
  increase the spacing, or expand the canvas.
- Preserve the UML arrowhead, direction, stereotype, and condition associated
  with the relationship; only the connector geometry changes.

Canonical draw.io style fragment:

~~~xml
style="edgeStyle=none;rounded=0;orthogonalLoop=0;html=1;curved=0;..."
~~~

Canonical geometry for a straight connection has no waypoint array:

~~~xml
<mxGeometry relative="1" as="geometry"/>
~~~

When several straight connectors converge, use draw.io connection anchors such
as `entryX`, `entryY`, `exitX`, and `exitY` to distribute their endpoints.
Give converging generalizations distinct target anchors so their hollow
triangles remain separate. Anchors select attachment points; they do not create
intermediate waypoints or relax the straight-line requirement.

### UML relationship notation

Use only standard UML relationship notation. A connector label must not invent
an implementation-specific stereotype.

| Relationship | Required notation and direction |
| --- | --- |
| Actor association | Solid line with no arrowhead and no stereotype |
| Include | Dashed dependency with an open arrowhead from the including use case to the included use case; label exactly `«include»` |
| Extend | Dashed dependency with an open arrowhead from the extending use case to the base use case; label exactly `«extend»` |
| Generalization | Solid line with an unfilled triangular arrowhead pointing to the more general actor or use case |

Show an extension condition separately as a guard such as `[type = itsi]`; do
not append it to the `«extend»` stereotype. Do not use pseudo-stereotypes such
as `«dispatch»`, `«requires»`, `«publish»`, or `«provision/use»`.

Actor generalization means **is a kind of**. Do not use it to represent physical
containment. For example, Standalone Runtime and C3 Runtime may specialize an
abstract Splunk Runtime actor, but they must not specialize a Kubernetes
Platform actor merely because Kubernetes hosts them. Show the physical
Kubernetes, vCluster, namespace, and workload nesting in a UML deployment
diagram.

### Use-case decomposition and lifecycle

- A broad use case may `«include»` a focused internal use case when the focused
  behavior is required and reusable.
- Connect a supporting actor or resource to the narrowest use case that
  actually consumes it, rather than only to an umbrella workflow.
- Use placement to show a natural lifecycle such as reference or configure,
  enable, and monitor.
- Do not add `«include»` merely to express chronological order between
  independently initiated use cases.

### Use-case naming

- Name each use case with a concise verb-object phrase.
- Prefer two to five words and no more than two rendered lines.
- Prefer responsibility or outcome language such as **Reconcile ITSI** over
  implementation-mechanism language such as **Prepare Payload**.
- Keep meaningful domain scope such as ITSI or SSAi explicit. Mention version
  selection in the name only when selecting or referencing a version is itself
  the goal; put concrete version numbers in a note.
- Put concrete version values, status codes, fields, digests, hashes, restart
  mechanics, and topology-specific implementation details in a note rather
  than an ellipse.
- Do not connect independently initiated status or inspection use cases with
  `«include»` merely to show that one follows another in time.

### Draw.io boundary ownership

An element semantically contained by a system or subject boundary must be a
real child of that boundary in the editable draw.io source, not merely a
root-level element visually overlapping the rectangle.

- Give the boundary `container=1;collapsible=0;recursiveResize=0;`.
- Set each contained element's `parent` to the boundary cell ID.
- Store contained-element geometry in boundary-local coordinates.
- Keep external actors and other elements outside the subject at the root
  diagram level.

This preserves semantic ownership and keeps the contents attached when the
boundary is moved or resized.

## Deployment diagram organization

Unless the current user request explicitly overrides them, apply the following
rules to every deployment diagram.

### Viewpoint and abstraction

- Give each deployment diagram one explicit viewpoint, such as runtime
  topology, build and publication, or operational workflow. Do not combine
  these viewpoints into one diagram.
- A runtime deployment diagram should answer **where things run, which
  artifacts are deployed there, and which runtime communication paths are
  significant**.
- If build-pipeline, provisioning, runtime, and troubleshooting views are all
  required, create separate diagrams for them instead of adding every detail to
  one canvas.
- Keep labels concise: use the standard UML stereotype, a clear component name,
  and at most one short qualifier.
- Put versions, hashes, paths, state codes, retry phases, validation evidence,
  and detailed workflow mechanics in accompanying documentation or a narrowly
  scoped note rather than in the primary deployment symbols.

### Layout and physical containment

- Arrange external sources, clients, repositories, and services on the left
  and the nested target runtime on the right.
- Prefer a wide landscape canvas when the target has several containment
  levels. Expand the canvas rather than compressing the hierarchy.
- Model physical containment explicitly using the appropriate hierarchy, for
  example:

      node → execution environment → namespace or runtime → artifact

- Make every nested element a real draw.io child of its enclosing deployment
  element, following **Draw.io boundary ownership** above.
- Boundary placement expresses physical or runtime containment, not
  organizational ownership. A provisioner-managed external resource, such as
  an S3 bucket, remains outside the Kubernetes node when it does not execute
  inside that cluster.
- Show alternative deployment topologies in one boundary only when they are
  clearly marked as mutually exclusive, for example **DEPLOY ONE TOPOLOGY**.

### UML notation and artifact lineage

- Use 3D boxes for UML nodes and devices, rectangles for execution
  environments, folded pages for artifacts, and solid lines for communication
  paths.
- Place each deployed artifact inside the node or execution environment that
  receives or runs it.
- Show enough artifact lineage to identify its source, deployed destination,
  and any deployment-significant propagation between runtimes.
- Include internal propagation only when it materially explains the deployed
  topology, such as a Cluster Manager distributing a bundle to indexers or an
  SHC Deployer distributing a bundle to search heads.
- Label communication paths with a short action and, when useful, the protocol,
  for example **Fetch apps / HTTPS**. Avoid multi-clause operational
  instructions on connectors.

### Color and change semantics

Use the following shared palette for Kraken, SOK, and ITSI architecture
diagrams:

| Color | Meaning |
| --- | --- |
| Blue | Kraken |
| Teal | Existing SOK or Splunk deployment |
| Orange | New or extended ITSI behavior and artifacts |
| Gray | External, stock, unchanged, or contextual elements |

- Apply the palette consistently to symbols, labels, and related connectors.
- Do not rely on color alone. Reinforce the distinction with short textual
  qualifiers such as **EXTERNAL**, **STOCK / UNCHANGED**, or
  **EXISTING / EXTENDED**.

## Component diagram organization

Unless the current user request explicitly overrides them, apply the following
rules to every component diagram.

### Viewpoint and deployment traceability

- Build each component diagram as a zoom into exactly one named box from a
  deployment diagram.
- Identify the decomposed deployment box in the component diagram title or
  subtitle.
- Show only systems that interact directly with the zoomed box. Omit transitive
  systems that have no direct runtime interaction with it.
- Every exposed inbound or outbound interaction must trace to a communication
  path in the deployment diagram.
- Reuse the deployment diagram's component names, runtime names, interaction
  terminology, and color semantics.
- Create a separate component diagram when another deployment box must be
  decomposed. Do not combine the internals of several deployment boxes into one
  component boundary.

### Subsystem boundary facades

- Represent every direct deployment-level interaction using a named square
  facade port on the zoomed subsystem boundary.
- Do not bypass the subsystem boundary with a direct connection between an
  internal component and an external collaborator.
- Place upstream and control-plane collaborators on the left of the subsystem
  and downstream runtime targets on the right.
- Keep facade names short, responsibility-oriented, and implementation-aligned,
  for example `appRepository`, `cmAppStage`, or `shcSetup`.
- Put the action and protocol on the connector rather than in the facade name.
- A facade port is a trace point for a deployment interaction, not an additional
  implementation component.

The canonical facade-port geometry is:

| Property | Requirement |
| --- | --- |
| Width | 30 |
| Height | 30 |
| Ratio | 1:1 |
| Placement | Centered across the subsystem boundary |

### Component selection and granularity

- Emphasize new and changed components. Show untouched components only where
  they are needed to explain an interface, dependency, or integration point.
- Define a component by a cohesive responsibility, lifecycle, and interface,
  not by an individual function.
- Treat a shared key file as a consolidation clue, not an absolute rule.
  Combine responsibilities only when their behavior and interfaces are also
  cohesive.
- Keep topology-specific responsibilities separate when they expose different
  contracts or have distinct lifecycle behavior.
- Avoid both method-per-component fragmentation and one oversized extension
  component.
- Tie every new component to the existing components it extends or consumes.
  Do not leave a new component visually isolated from its integration point.
- Use implementation-accurate component names, interface names, provider and
  consumer roles, and file paths.

Each component box should contain only:

1. `«component»`
2. A concise component name
3. One short, clear **Does:** sentence
4. One **Key File:** path

Do not include `Takes:`, `Provides:`, method inventories, status codes, retry
phases, or detailed workflow mechanics in the component box. Put necessary
implementation detail in a nearby note or accompanying documentation.

### Interface selection and fidelity

Choose the relationship notation from the modeled contract rather than from
the desired visual appearance:

| Situation | Required notation |
| --- | --- |
| Stable interface implemented in code | Ball/socket using the exact code interface name |
| Intentionally frozen seam without a code interface | `«design interface»` plus a one-to-one mapping to the current call seam |
| Helper call or ordinary implementation dependency | Dashed open-arrow `«use»`, directed client to supplier |
| External API or protocol without a modeled formal interface | Dashed `«use»` with a concise action and optional protocol |
| Untyped component-to-component relationship | Not permitted |

- Use ball-and-socket notation for a formal, stable, or intentionally frozen
  provided/required contract, especially an inbound or required interface of a
  new component.
- Use the exact implementation interface name when one exists.
- If the implementation has only a call seam but the design intentionally
  freezes it as a contract, label it `«design interface»` and map it one-to-one
  to the current function in a small note.
- Do not model an existing formal interface merely as
  `«use» InterfaceName`.
- Do not invent a formal interface for an external service when the
  implementation only uses a protocol or service API. Model that interaction
  as a named `«use»` dependency unless the API itself is intentionally part of
  the component model.
- Place the provided ball at the provider and the required socket at the
  consumer. Connect them with a solid, arrowless assembly connector.
- If one provided contract serves several consumers, repeated lollipops may be
  used only when they materially improve route legibility. Label the contract
  once and state in the legend that the repeated symbols represent the same
  interface.
- Never use a bare, untyped solid join between components. Every relationship
  must be an interface assembly or a named UML dependency such as `«use»`.

### Ball-and-socket geometry

Use the following canonical horizontal interface geometry:

| Symbol | Width | Height | Requirement |
| --- | ---: | ---: | --- |
| Provided ball | 24 | 24 | 1:1 and `aspect=fixed` |
| Required socket | 34 | 34 | 1:1 and `aspect=fixed` |
| Provider or consumer stem | approximately 20 | 2 | Short and visibly rendered |

- The provided ball must be close to the provider but must not sit directly on
  the component boundary.
- Give the ball a short lollipop stem. Do not lengthen the provider stem merely
  to reach a distant consumer.
- The required socket belongs on the consumer side. The socket-side assembly
  connector may span the longer distance between components.
- Mirror or rotate the complete ball, socket, mask, and stem construction when
  orientation changes. Do not distort any symbol's proportions.
- Verify that short stems are visible in both PNG and SVG renders, not only in
  the draw.io XML.
- If draw.io suppresses a very short zero-label edge during export, represent
  the stem as a non-connectable thin rectangle using the interface color. The
  rectangle is part of the interface glyph, not an untyped component join.

### Connector semantics and routing

- A straight geometric route is allowed when it is readable. The prohibited
  case is an untyped bare join, not straightness itself.
- Every solid arrowless edge must be an intentional ball/socket assembly
  connector.
- Use a dashed open-arrow `«use»` dependency for helper calls and other
  dependencies that are not modeled as stable interfaces. Its direction is
  client to supplier.
- Label an external dependency with a concise action and, when useful, its
  protocol, for example **Fetch apps / HTTPS** or
  **pods/exec • Install apps**.
- Route long assembly connectors and dependencies through dedicated whitespace
  gutters.
- Do not route through component text, interface labels, facade ports, notes,
  boundary headings, or component symbols.
- Follow the general non-overlap rule in **Connector routing** below: parallel,
  visibly separated routes are preferable to coincident segments, and a clear
  crossing is preferable to overlapping lines.

### Component-level change semantics

Use the shared palette with the following component-level refinement:

| Appearance | Meaning |
| --- | --- |
| Teal fill and teal outline | Existing unchanged SOK or Splunk component |
| Teal fill and orange outline | Existing component modified for ITSI |
| Orange fill and orange outline | New ITSI component or contract |
| Gray | External, stock, unchanged, or contextual component |

- Apply the same semantic color to related ports, interface symbols, labels,
  and connectors.
- Reinforce color with a legend or short text because color must not be the
  only indicator of change.
- State explicitly when new logical components compile into an existing image
  or process and do not introduce a new pod, service, sidecar, controller,
  image, or deployment node.

### Component diagram validation

Before delivering a component diagram:

1. Confirm the diagram decomposes exactly one named deployment box.
2. Confirm every exposed facade maps to a direct communication path in the
   deployment diagram.
3. Confirm indirect systems without a direct runtime interaction are omitted
   or explicitly identified as contextual.
4. Confirm new and changed components are emphasized and untouched components
   are limited to necessary integration points.
5. Confirm each component has a cohesive responsibility and contains only its
   stereotype, concise name, **Does:** sentence, and **Key File:** path.
6. Confirm every important formal or frozen requirement of a new component
   uses ball-and-socket notation.
7. Confirm every `«interface»` exists in the implementation and every
   `«design interface»` maps to a real call seam.
8. Confirm provider, consumer, interface, component, and file names match the
   implementation.
9. Confirm there are no bare solid component joins and every arrowless solid
   edge is intentionally an assembly connector.
10. Confirm provided balls have short visible stems and do not sit directly on
    provider boundaries.
11. Confirm ball, socket, and facade-port proportions are preserved.
12. Confirm no connectors share coincident segments and long routes use
    dedicated gutters.
13. Confirm connector routes and labels do not cover symbols, component text,
    interface labels, ports, notes, or boundary headings.
14. Confirm the change palette is explained in a legend and does not imply a
    new deployment unit when the code compiles into an existing image.
15. Confirm all draw.io XML IDs are unique.
16. Inspect dense interface areas in both PNG and SVG renders.

## Spacing requirements

The approved layout uses deliberately generous whitespace. Do not make a
diagram fit by shrinking symbols or reducing these gaps. Expand the canvas and
containers instead.

### Canonical edge-to-edge gaps

| Relationship | Final gap |
| --- | ---: |
| Adjacent use cases in a standard two-column boundary | 380 px |
| Adjacent use cases in the four-column ITSI row | 420 px |
| Top row to middle row | 110 px |
| Middle row to bottom row | 80 px |
| ITSI top row to ITSI bottom row | 150 px |
| Kraken boundary to SOK boundary | 120 px |
| Current-system layer to ITSI extension boundary | 200 px |
| External actor to nearest system boundary | at least 140 px |
| System/extension boundary to footer | 80 px |

These values are the doubled gaps accepted in the final review:

| Gap | Previous | Final |
| --- | ---: | ---: |
| Standard horizontal | 190 px | 380 px |
| ITSI horizontal | 210 px | 420 px |
| Top-to-middle vertical | 55 px | 110 px |
| Middle-to-bottom vertical | 40 px | 80 px |
| ITSI row vertical | 75 px | 150 px |
| Boundary gutter | 60 px | 120 px |
| Current-to-ITSI layer | 100 px | 200 px |
| Boundary-to-footer | 40 px | 80 px |

### Boundary padding

Use these minimum internal clearances:

| Clearance | Minimum |
| --- | ---: |
| Boundary top to first symbol row | 90 px |
| Boundary side to first symbol | 120 px |
| Last symbol to boundary bottom | 20 px |

If a connector or label needs the clearance, enlarge the boundary instead of
placing content against its edge.

### Connector routing

- Route connectors through the whitespace created by the spacing rules.
- Keep cross-system dependencies in dedicated gutters.
- Never place two connectors directly on top of one another. Give related
  connectors distinct anchors and route them through visibly separated,
  parallel lanes.
- Crossings are acceptable when unavoidable, but coincident or overlapping
  connector segments are not.
- Do not route through symbols, actor labels, boundary titles, or notes.
- For use-case diagrams, use the straight connectors defined in
  **Straight connector requirement** above.
- For other UML diagram types, prefer short orthogonal routes when they improve
  readability.
- When adding symbols, expand and reroute the diagram; do not collapse the
  approved gaps.

## Approved use-case reference layout

The coordinates below describe the existing detailed reference diagram. For
new or reorganized use-case diagrams, the actor placement, two-column layout,
absence of a lane divider, and straight-connector requirements above are
mandatory. Expand or transpose this reference geometry as needed instead of
violating those requirements.

The final Kraken/SOK/ITSI use-case diagram uses:

| Element | Geometry |
| --- | --- |
| Canvas | 3300 × 2200 |
| Kraken boundary | 1200 × 780 at (380, 230) |
| SOK boundary | 1200 × 780 at (1700, 230) |
| ITSI boundary | 2520 × 720 at (380, 1210) |

Reference symbol coordinates:

| Area | X coordinates | Y coordinates |
| --- | --- | --- |
| Kraken two-column rows | 500, 1120 | 320, 590; lifecycle at 830 |
| SOK two-column rows | 1820, 2440 | 320, 590, 830 |
| ITSI four-column row | 500, 1160, 1820, 2480 | 1300 |
| ITSI two-symbol row | 1070, 1930 | 1610 |

All use cases at those coordinates are 240 × 160. The role-placement note
begins at y=1840, and the ITSI boundary ends at y=1930.

Use these exact coordinates when extending the approved diagram. For a new
diagram, preserve the ratios and minimum edge-to-edge gaps even if the absolute
coordinates differ.

## Validation checklist

Before delivering a diagram:

1. Confirm every actor has the canonical geometry and aspect=fixed.
2. Confirm every use-case ellipse is 3:2 and has aspect=fixed.
3. Confirm rectangles and containers are not unnecessarily aspect-locked.
4. Confirm external actors are on the far left and internal or downstream
   actors are on the far right.
5. Confirm the use cases form two vertical columns: external-facing on the left
   and internal or downstream on the right.
6. Confirm there is no vertical separation line between the two columns.
7. Confirm every use-case diagram connection is a straight line with no
   orthogonal edge style or intermediate waypoints.
8. Confirm actor associations, `«include»`, `«extend»`, and generalizations use
   the standard notation and direction defined above.
9. Confirm no pseudo-stereotypes appear and extension guards are separate from
   `«extend»`.
10. Confirm each actor has a distinct responsibility and duplicate actor aliases
    have been consolidated.
11. Confirm every actor hierarchy has the general actor above aligned sibling
    specializations and uses generalization only for **is a kind of**.
12. Confirm supporting actors connect to the narrowest consuming use cases.
13. Confirm use-case names are concise, outcome-oriented verb-object phrases
    with at most two rendered lines.
14. Confirm independently initiated lifecycle steps are ordered by placement,
    not artificial `«include»` relationships.
15. Confirm converging straight connectors use distinct anchors and do not have
    overlapping arrowheads.
16. Confirm every element inside a system boundary is a real child of that
    boundary in the draw.io source.
17. Measure gaps edge to edge.
18. Confirm connectors use whitespace and do not cross symbols or labels.
19. Validate the XML:

   ~~~bash
   xmllint --noout diagram.drawio
   ~~~

20. Render both PNG and SVG previews.
21. Inspect the rendered image at normal zoom.
22. Update the .drawio, .png, and .svg artifacts together.

Useful audits:

~~~bash
# These commands should produce no output.
rg 'shape=umlActor' diagram.drawio | rg -v 'aspect=fixed'
rg 'style="ellipse;' diagram.drawio | rg -v 'aspect=fixed'

# A use-case diagram should not contain orthogonal routing or waypoint arrays.
rg 'orthogonalEdgeStyle|Array as="points"' use-case-diagram.drawio

# A use-case diagram should not contain a lane-divider element.
rg 'column-divider|lane-divider' use-case-diagram.drawio

# A use-case diagram should not contain implementation-specific pseudo-stereotypes.
rg '«(dispatch|requires|publish|provision/use)»' use-case-diagram.drawio

# For the canonical use-case template, every ellipse should match.
rg -A1 'style="ellipse;' diagram.drawio |
  rg 'width="240" height="160"'
~~~

## Precedence

1. The current user's explicit requirements.
2. This guidance.md.
3. Existing sample diagrams.
4. General draw.io defaults.

If an older sample conflicts with this document, follow this document.
