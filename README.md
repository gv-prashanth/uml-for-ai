# uml-for-ai

A practical guide for AI agents to create clear, consistent, and high-quality UML diagrams.

This file is the source of truth for UML notation, diagram organization,
symbol geometry, connector routing, and spacing in diagrams created from this
repository.

**Read this file before creating or modifying a diagram.** Explicit instructions
in the current user request take precedence; otherwise, apply these rules
without asking for confirmation.

## Quick guide for selecting UML diagrams

### Structural UML diagrams

Use structural diagrams as a progressive zoom:

**Deployment Diagram → Component Diagram → Class Diagram**

- **Deployment Diagram / System Diagram:** Shows the complete deployment and
  system architecture of a complex system.

- **Component Diagram:** Zooms into an individual subsystem or box from the
  deployment diagram. Usually, create one component diagram for one execution
  environment. Create multiple component diagrams when different subsystems
  need to be explained separately.

- **Class Diagram:** Zooms into an individual component within a subsystem.
  This is the lowest level of structural detail.

- **Domain Model Diagram:** Use when the focus is only on the main domain
  objects. It is similar to a class diagram but shows domain classes rather
  than behavioral or implementation classes.

Other structural UML diagram types are generally not needed.

### Behavioral UML diagrams

- **Use-Case Diagram:** Use when first understanding a system from its product
  and use-case perspective. It shows the actors, use cases, and dependencies
  between use cases.

- **Activity Diagram:** A flowchart-like view used to show the workflow of a
  single component.

- **Communication Diagram:** Shows a system workflow at a high level and
  explains how different components interact by calling their provided and
  required interfaces.

- **Interaction Overview Diagram:** Combines activity and communication views
  to show the workflow across two or more components, without describing the
  complete system.

- **Sequence Diagram:** Gives a detailed explanation of the workflow across
  two or more components, with a strong focus on time and call sequence.

- **State Diagram:** Shows state or configuration transitions for something
  maintained in memory.

- **Timing Diagram:** Do not use. Prefer the other behavioral diagrams instead.

### Data diagram

- **Entity-Relationship Diagram:** Shows entity relationships, especially for
  data stored in a database.

## Reference examples

The editable draw.io sources are in [`examples/src`](examples/src); matching
static renders are in [`examples/images`](examples/images). Product names in
these examples are illustrative—the guidance above remains the source of
truth.

- [C3 current component](examples/src/c3-notification-service-processor-component.drawio)
  and [proposed component](examples/src/c3-notification-service-processor-component-itsi-proposed.drawio)
- [C3 current deployment](examples/src/c3-actions-dataflow-deployment.drawio)
  and [proposed deployment](examples/src/c3-actions-dataflow-deployment-proposed.drawio)
- [C3 domain-object field mapping](examples/src/c3-open-itsi-domain-mapping-class.drawio)
- [O11y current component](examples/src/o11y-incident-service-grouping-component.drawio)
  and [proposed component](examples/src/o11y-incident-service-grouping-component-openitsi.drawio)
- [O11y domain-object field mapping](examples/src/o11y-open-itsi-domain-mapping-class.drawio)
- [Standalone ownership use case](examples/src/open-itsi-standalone-use-case.drawio)
- [Interface domain model](examples/src/open-itsi-interface-domain-model-class.drawio)

## Evidence and claim discipline

Architecture diagrams are evidence-backed claims, not inventories of things
that happen to exist in a repository.

- Draw a current or as-built relationship only when consumer-side code,
  configuration, IaC, tests, or runtime evidence proves it. The existence of
  an endpoint, class, bundled module, or library does not prove that another
  element uses it. A proposed relationship requires design or requirement
  evidence, an explicit **proposed** status, and visible TBDs for unresolved
  contracts.
- Keep a small evidence snapshot beside the editable diagram. For each
  important node or relationship, record the modeled claim, its source,
  reviewed revision or date, and claim status.
- Distinguish **verified current**, **proposed**, **unknown / TBD**, and
  **explicitly absent or out of scope**. Use separate pages or diagrams for
  current and proposed topology when combining them could imply that a roadmap
  design is already deployed.
- Participation does not imply direct access. Show every material mediator in
  a path such as client → service → repository → database; do not shorten that
  to client → database merely because both participate in the same capability.
- Distinguish durable authority, transport, cache or enrichment data, and
  executable artifacts. A queue, topic, or object store used for transport is
  not automatically the system of record.
- Distinguish packaged code that is executed from code merely present in an
  archive or image. Show hybrid ownership and immutable vendor boundaries
  precisely.
- Put detailed source mappings, negative findings, unresolved questions, and
  intentional exclusions in a companion evidence note rather than crowding
  the primary canvas.

## Comparative current and proposed diagrams

When a proposed architecture is derived from an existing current diagram,
create the proposed editable source by copying the current source first. Keep
the baseline stable so the two diagrams can be compared directly.

- Preserve unchanged component and interface IDs, names, positions,
  dimensions, styles, relationships, and overall layout.
- Add only the proposed components, connectors, and changes. Do not reorganize
  or restyle unrelated parts of the current architecture in the proposed view.
- Emphasize new or changed elements with a documented accent color, bold
  treatment, or another visible change marker. Use a legend and do not rely on
  color alone.
- If the baseline must also change, make that a separately identified current
  diagram revision rather than hiding the baseline change in the proposed view.

This copy-and-diff approach applies to deployment, component, use-case, class,
and domain-model diagrams whenever the purpose is to explain an architectural
change.

## Semantic ownership and delegated behavior

When a system hosts, adapts, or exposes an external engine or package,
distinguish runtime ownership from semantic ownership.

- Show the external engine or package as an external actor or collaborator
  when it owns the business meaning or decision logic.
- Show only the use cases and responsibilities performed by the subject
  system. Represent delegated business logic as an interaction with the
  external authority rather than as behavior owned by the subject.
- Do not imply that an adapter, runtime host, wrapper, or transport component
  owns business rules merely because it starts, packages, or provides access
  to the implementation.
- Keep the ownership boundary visible in the diagram and label the external
  authority clearly when the distinction affects interpretation of the design.

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
- Do not promote a database, queue, cache, feature-flag service, or other
  implementation mechanism to an actor unless it genuinely plays an external
  role at the subject boundary. Connect infrastructure in the deployment,
  component, communication, or data-model viewpoint where it belongs.
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

Show an extension condition separately as a guard such as `[mode = advanced]`; do
not append it to the `«extend»` stereotype. Do not use pseudo-stereotypes such
as `«dispatch»`, `«requires»`, `«publish»`, or `«provision/use»`.

Actor generalization means **is a kind of**. Do not use it to represent physical
containment. For example, a standalone runtime and a clustered runtime may
specialize an abstract application-runtime actor, but they must not specialize
a container-platform actor merely because that platform hosts them. Show the
physical compute node, execution environment, namespace, and workload nesting
in a UML deployment diagram.

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
- Prefer responsibility or outcome language such as **Reconcile Configuration** over
  implementation-mechanism language such as **Prepare Payload**.
- Keep meaningful domain or capability scope explicit. Mention version selection
  in the name only when selecting or referencing a version is itself the goal;
  put concrete version numbers in a note.
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

- Use a reader-oriented **left-to-right deployment layout**. Place user-facing
  actors, client devices, browsers, and presentation surfaces on the far left.
  Progress through edge/API, serving, application, persistence, processing,
  and integration layers toward source controllers, external data platforms,
  and other downstream systems on the far right.
- **For deployment diagrams only**, use this project convention for directed
  communication paths: **A → B means A uses or invokes B**. This convention
  does not alter the relationship notation defined for use-case, component, or
  other UML diagram types.
- A response may return on the same deployment interaction; do not reverse the
  arrow merely to show returned data. Preserve the use/invocation direction
  even when it runs right to left, such as a downstream controller invoking a
  platform ingress endpoint to publish an event. Placement communicates
  deployment reading order; the arrowhead identifies which endpoint uses or
  invokes the other.
- When a deployment viewpoint has no user-facing participant, place the
  initiating or upstream system on the left and the consumed/downstream runtime
  on the right.
- Prefer a wide landscape canvas when the target has several containment
  levels. Expand the canvas rather than compressing the hierarchy.
- Model physical containment explicitly using the appropriate hierarchy, for
  example:

      node → execution environment → namespace or runtime → artifact

- Make every nested element a real draw.io child of its enclosing deployment
  element, following **Draw.io boundary ownership** above.
- Boundary placement expresses physical or runtime containment, not
  organizational ownership. An externally managed storage resource remains
  outside a compute node or cluster when it does not execute there.
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
  topology, such as a control node distributing an artifact to worker nodes.
- Label communication paths with a short action and, when useful, the protocol,
  for example **Fetch artifact / HTTPS**. Avoid multi-clause operational
  instructions on connectors.

### Color and change semantics

Define a consistent semantic palette for each diagram. A practical starting
point is:

| Role | Suggested treatment |
| --- | --- |
| Existing or current architecture | Blue or teal |
| New or modified behavior | Orange or another accent color |
| External, unchanged, or contextual elements | Gray |

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
  for example `artifactRepository`, `deploymentStage`, or `runtimeSetup`.
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
| Any interaction between subcomponents | Native draw.io Provided/Required Interface assembly with an adjacent exact code interface or implementation method name |
| Stable interface implemented in code | Native draw.io Provided/Required Interface assembly using the exact code interface name |
| Call seam without a formal code interface | Native draw.io Provided/Required Interface assembly labeled `«design interface»`, named for the provider or contract, and mapped to the exact implementation method or cohesive operation set |
| Connection from an internal component to its subsystem facade port | Solid arrowless UML delegation connector terminating at the named port |
| Interaction with an external API, protocol, or system | Named facade port plus dashed/dotted `«use»` with a concise action and optional protocol |
| Untyped component-to-component relationship | Not permitted |

- Model every interaction between subcomponents with draw.io's native
  **Provided/Required Interface** symbol (`shape=providedRequiredInterface`).
  This requirement also applies to helper calls and ordinary implementation
  dependencies; do not use dashed/dotted `«use»` between subcomponents.
- Place an adjacent label beside every native ball/socket instance. Use the
  exact implementation interface name when one exists.
- If no formal interface exists, label the seam `«design interface»` and place
  the exact implementation method name adjacent to the native symbol. One
  symbol may list several methods only when they form one cohesive contract
  between the same provider and consumer; map every listed operation to code
  in a small note.
- Do not model an existing formal interface merely as
  `«use» InterfaceName`.
- Use a dashed/dotted `«use»` dependency only for an interaction with an
  external API, protocol, or system. Route it through the named facade port
  required by **Subsystem boundary facades**.
- Make the facade port an actual XML endpoint of the external dependency; do
  not merely draw the dependency across the port. Connect the relevant
  internal component to that port with a solid arrowless UML delegation
  connector. This component-to-port connector is the only permitted solid
  arrowless exception to a ball/socket assembly.
- Do not invent a formal interface for an external service when the
  implementation only uses a protocol or service API. Model that interaction
  through a named facade port as a named `«use»` dependency.
- Place the provided side of the native symbol toward the provider and the
  required side toward the consumer. Connect both sides with solid, arrowless
  assembly connectors.
- If one provided contract serves several consumers, repeated native symbols
  may be used only when they materially improve route legibility. Put the same
  exact interface or implementation method name adjacent to every repeated
  instance, and state in the legend that they represent the same interface.
- Never use a bare, untyped solid join between components. Every interaction
  between subcomponents must be a native interface assembly; only external
  interactions may use a named `«use»` dependency.

### Provided/Required Interface geometry

Use the following canonical horizontal interface geometry:

| Symbol | Width | Height | Requirement |
| --- | ---: | ---: | --- |
| Native Provided/Required Interface | 34 | 34 | `shape=providedRequiredInterface` and 1:1 |
| Provider or consumer stem | approximately 20 | 2 | Short and visibly rendered |

Canonical draw.io style fragment for a socket-left, ball-right orientation:

~~~xml
style="shape=providedRequiredInterface;html=1;verticalLabelPosition=bottom;sketch=0;flipH=1;inset=3;fillColor=...;strokeColor=...;strokeWidth=2;"
~~~

- Use one native `providedRequiredInterface` cell for each symbol. Do not
  reconstruct it from separate ellipses, masking rectangles, or overlapping
  layers.
- The provided side must be close to the provider but must not sit directly on
  the component boundary.
- Give the provided side a short lollipop stem. Do not lengthen the provider
  stem merely to reach a distant consumer.
- The required side belongs on the consumer side. Its assembly connector may
  span the longer distance between components.
- Use `flipH=1`, `flipV=1`, or rotation on the single native symbol when its
  orientation changes. Keep the geometry square and do not distort it.
- Verify that short stems are visible in the PNG render, not only in the
  draw.io XML.
- If draw.io suppresses a very short zero-label edge during export, represent
  the stem as a non-connectable thin rectangle using the interface color. The
  rectangle is part of the interface glyph, not an untyped component join.

### Connector semantics and routing

- A straight geometric route is allowed when it is readable. The prohibited
  case is an untyped bare join, not straightness itself.
- Every solid arrowless edge must be either an intentional ball/socket assembly
  connector or a UML delegation connector from an internal component to a
  named facade port.
- Do not use a dashed/dotted `«use»` dependency for helper calls or any other
  interaction between subcomponents. Use a native Provided/Required Interface
  assembly labeled with the exact interface or implementation method name.
- A dashed/dotted open-arrow `«use»` dependency is permitted only for an
  external interaction. Its direction is client to supplier, and the named
  subsystem facade port must be an actual XML endpoint rather than a symbol
  that the dependency merely crosses.
- Label an external dependency with a concise action and, when useful, its
  protocol, for example **Fetch artifact / HTTPS** or
  **Submit request / HTTPS**.
- Route long assembly connectors and dependencies through dedicated whitespace
  gutters.
- Do not route through component text, interface labels, facade ports, notes,
  boundary headings, or component symbols.
- Follow the general non-overlap rule in **Connector routing** below: parallel,
  visibly separated routes are preferable to coincident segments, and a clear
  crossing is preferable to overlapping lines.

### Dense component diagrams: expand the routing field

A dense wire field is a layout defect even when every component and
relationship is architecturally correct. At the intended viewing scale, a
reader must be able to start at either endpoint of any connector and follow the
entire route without losing it in another line, label, symbol, or boundary.

Do not solve density by shrinking component boxes or text, stacking connectors
on the same segment, or adding arbitrary bends. Increase the space available
to the relationships while preserving the semantic model.

#### Layout-preserving expansion

When the components and interactions are already correct, treat the redesign
as a geometry-only refactor:

- Preserve component IDs, names, responsibilities, dimensions, styles,
  interface names, facade names, relationship types, and source/target pairs.
- Preserve the canonical dimensions of component boxes, native interface
  symbols, facade ports, text, and notes. Do not make the symbols smaller to
  create the appearance of more space.
- Enlarge the page, subsystem boundary, and semantic bands; then increase the
  edge-to-edge gaps between components.
- Change only positions, container dimensions, anchors, waypoints, and label
  geometry unless an architectural change was explicitly requested.
- Compare the semantic signature before and after the refactor. A layout-only
  change must not add, remove, redirect, rename, or retype a dependency.

#### Plan routing corridors before routing lines

Identify high-degree components and long cross-band relationships before
placing connectors. Reserve whitespace for the routes themselves:

- Use a **fan-out terrace** between a consumer row and a provider row. Give
  each source-to-target route a distinct horizontal track, then align its final
  vertical segment with the target interface.
- Use an **inter-band corridor** for relationships that cross responsibility
  bands, rather than weaving those lines through intermediate components.
- Use **side gutters** for control-plane and external-system paths, and a
  **lower or upper gutter** for long sibling calls that would otherwise cross
  a row of components.
- Give every parallel route a distinct anchor and track. Interleave tracks
  from multiple fan-out sources when that produces a clearer terrace.
- Reserve clearance for connector labels as part of the corridor. A line that
  is clear but whose label covers another line or symbol is still a failed
  route.
- Prefer a visible perpendicular crossing over coincident segments. A crossing
  can be followed; two connectors occupying the same track cannot.

#### Use the smallest export-safe canvas that is readable

Canvas expansion is a means, not the goal. Expand until all routes are
traceable at the intended normal zoom, but avoid large unused regions. Very
large draw.io pages can exceed browser or Chromium tile-memory limits and
export with clipped, blank, or missing regions.

- Export at the intended scale after every major expansion instead of waiting
  until the end.
- Treat renderer tile or memory warnings, blank tiles, clipped bands, and
  missing symbols, stems, or labels as validation failures.
- If an export fails, compact unused whitespace and oversized corridors while
  preserving component, interface, facade-port, and text dimensions.
- Re-run the full-page inspection after compaction; a successful half-scale
  export does not prove that the intended full-scale artifact is valid.

### Component-level change semantics

Use the diagram's semantic palette with the following component-level
refinement:

| Appearance | Meaning |
| --- | --- |
| Base fill and base outline | Existing unchanged component |
| Base fill and accent outline | Existing modified component |
| Accent fill and accent outline | New component or contract |
| Gray | External, stock, unchanged, or contextual component |

- Apply the same semantic color to related ports, interface symbols, labels,
  and connectors.
- Reinforce color with a legend or short text because color must not be the
  only indicator of change.
- State explicitly when new logical components compile into an existing image
  or process and do not introduce a new runtime unit, auxiliary process,
  endpoint, image, or deployment node.

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
6. Confirm every interaction between subcomponents uses the native draw.io
   Provided/Required Interface symbol.
7. Confirm every native ball/socket instance has an adjacent exact interface
   or implementation method name or cohesive operation set from the code.
8. Confirm every `«interface»` exists in the implementation and every
   operation named on a `«design interface»` maps to code. If one symbol lists
   several operations, confirm they share the same provider, consumer, and
   cohesive contract.
9. Confirm no dashed/dotted `«use»` connects subcomponents; every `«use»`
   represents an external interaction and uses a named facade port as an
   actual XML endpoint.
10. Confirm provider, consumer, interface, component, and file names match the
   implementation.
11. Confirm there are no bare solid component joins and every arrowless solid
    edge is intentionally either an assembly connector or a delegation
    connector from an internal component to a named facade port.
12. Confirm provided sides have short visible stems and do not sit directly on
    provider boundaries.
13. Confirm native interface-symbol and facade-port proportions are preserved,
    with no separately layered ellipse-and-mask substitutes.
14. Confirm no connectors share coincident segments and long routes use
    dedicated gutters.
15. Confirm connector routes and labels do not cover symbols, component text,
    interface labels, ports, notes, or boundary headings.
16. Confirm the change palette is explained in a legend and does not imply a
    new deployment unit when the code compiles into an existing image.
17. Confirm all draw.io XML IDs are unique within each draw.io page.
18. Export at the intended scale, inspect the full-page PNG and dense interface
    areas, and confirm the renderer reports no tile or memory warnings and
    produces no clipped, blank, or missing regions.

## Class and domain-model diagram organization

Use class and domain-model diagrams to explain the important domain objects
that cross a system or component boundary, not every implementation class.

- Include a small representative JSON example beside each relevant class when
  the diagram explains an API, Kafka, or other wire contract. Mark the example
  as authoritative, illustrative, or protocol-specific.
- When comparing two systems, draw one connector for each material field-level
  mapping and make its direction explicit. Document renames, transformations,
  defaults, and omitted fields.
- Group related mappings and reserve whitespace so individual lines remain
  traceable. If field-level lines make the diagram too dense, keep the
  high-level mapping on the canvas and provide the complete mapping in a
  companion table.
- When the same domain concept crosses multiple interface types, prefer one
  canonical domain model with protocol-specific envelopes or adapters around
  it. Do not create divergent API and Kafka domain classes unless evidence
  requires different contracts.
- Keep implementation-only helpers, transport envelopes, and persistence
  details out of a domain-model view unless they are necessary to explain the
  boundary or mapping.

## Entity-relationship diagram organization

- State whether the diagram is a physical schema, logical domain model,
  derived read model, or migration target. Do not blend those viewpoints
  without explicit bands and a legend.
- Group entities by schema, authority, lifecycle, or durability boundary when
  those distinctions materially affect ownership or behavior.
- Visually distinguish and label these relationship classes:
  - declared primary keys and database-enforced foreign-key relationships;
  - application-managed references without a database constraint;
  - identifiers embedded in JSON or another opaque payload;
  - derived, projection, and read-time joins.
- Show cardinality only when constraints or implementation evidence support
  it. Do not infer `1`, `0..1`, or `*` from field names alone.
- Keep declared storage authority separate from compatibility projections,
  transport receipts, outboxes, caches, and materialized read models.
- Record the schema or migration revision and the evidence date. When a model
  is historical, mark it prominently and pair it with the replacement model
  rather than presenting it as current.
- For a genuinely multi-phase comparative design, prefer one page per phase
  plus a compact cumulative target page. Keep unchanged prior-phase elements
  muted and preserve stable positions so readers can compare phases without
  relearning the layout.
- Put invariants, deferrals, exit gates, ownership, and known scale risks near
  the relevant model without disguising them as relationships.

## Communication diagram organization

Use a communication diagram when the important question is which participants
exchange messages for one scenario while preserving the collaboration
structure.

- Give messages ordered sequence numbers such as `1`, `1.1`, and `2`, and use
  concise verb-object names. Put guards or conditions beside the message.
- Name the logical interface or operation when known. Add the transport only
  when it materially clarifies the contract.
- Show mediators explicitly. A client message to a service followed by a
  repository message to a database must not be collapsed into a direct
  client-to-database message.
- Separate request/coordination, transport, and persistence planes when they
  otherwise create ambiguous crossings.
- Use a directed numbered message for invocation. If a supplemental view shows
  only capability participation and not invocation order, use a clearly
  explained nondirectional participation link instead of inventing direction.
- Keep one scenario or tightly related scenario family per diagram. Use a
  sequence diagram when time ordering, activation, looping, or concurrency is
  the dominant concern.

## Interactive architecture companions

An interactive HTML explainer may accompany UML when progressive disclosure,
capability matrices, alternative journeys, or executive/detail views add
material value. It does not replace the editable UML source and static render.

- Preserve the same boundaries, names, directions, and claim statuses as the
  source diagram.
- Provide a readable no-JavaScript fallback and respect reduced-motion
  preferences.
- Validate keyboard access, responsive widths, JavaScript syntax, console
  errors, horizontal overflow, and all filters or view switches.
- Keep interaction-specific implementation files out of the canonical UML
  example set unless they are intentionally maintained as reusable tooling.

## Spacing requirements

Use deliberately generous and consistent whitespace. Do not make a diagram fit
by shrinking symbols or compressing relationships. Expand the canvas and
containers instead.

### Spacing hierarchy

- Measure every gap edge to edge.
- Use one consistent gap for peers in the same row or column.
- Make the gap between semantic groups visibly larger than the gap within a
  group.
- Reserve dedicated whitespace gutters for long connectors and converging
  relationships.
- Leave enough space between actors and system boundaries to make containment
  unambiguous.
- Reserve a clear header band inside every boundary and keep symbols away from
  its sides and bottom edge.
- If a connector or label consumes the available clearance, enlarge the
  boundary rather than placing content against its edge.

### Connector routing

- Route connectors through the whitespace created by the spacing hierarchy.
- Keep cross-system dependencies in dedicated gutters.
- Never place two connectors directly on top of one another. Give related
  connectors distinct anchors and route them through visibly separated,
  parallel lanes.
- Crossings are acceptable when unavoidable, but coincident or overlapping
  connector segments are not.
- Do not route through symbols, actor labels, boundary titles, or notes.
- At the intended normal zoom, verify that every route can be followed from
  source to target without selecting it or guessing where an overlapping
  segment continues.
- For use-case diagrams, use the straight connectors defined in
  **Straight connector requirement** above.
- For other UML diagram types, prefer short orthogonal routes when they improve
  readability.
- When adding symbols, expand and reroute the diagram; do not collapse the
  established spacing hierarchy.

## Page bounds and export framing

Size the draw.io page to the actual diagram content before exporting. Remove
unused grid rows, columns, and blank trailing regions that make the PNG appear
larger than the diagram.

- Page bounds are presentation framing, not architectural content.
- After resizing or restructuring a diagram, regenerate the PNG and inspect
  the complete export to confirm that no symbols, labels, connectors, or page
  content were clipped.
- Keep the smallest export-safe page that preserves the required whitespace
  and normal viewing scale.

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
17. For deployment diagrams, confirm user-facing surfaces are on the far left,
    downstream systems are on the far right, and every directed connector
    follows **A → B means A uses or invokes B**.
18. Confirm every important current relationship has consumer-side evidence
    and every proposed relationship has design or requirement evidence; record
    the claim status, and never treat existence alone as usage.
19. For ERDs, confirm relationship type and cardinality match database or code
    evidence, and that physical, logical, and derived relationships are
    visually distinguishable.
20. For communication diagrams, confirm messages are numbered, directed, and
    mediated through the participants that actually handle them.
21. For a current/proposed pair, confirm the proposed source preserves the
    unchanged baseline and visibly marks only the intended additions or changes.
22. Confirm delegated business logic is attributed to the correct external or
    internal authority and is not accidentally shown as subject-owned behavior.
23. For class or domain-model diagrams, confirm representative payload examples
    are classified and field-level mappings are complete or linked to a complete
    companion table.
24. Measure gaps edge to edge and confirm the page bounds do not include unused
    trailing grid space.
25. Confirm connectors use whitespace and do not cross symbols or labels.
26. Validate the XML:

   ~~~bash
   xmllint --noout diagram.drawio
   ~~~

27. Confirm every XML ID is unique within each draw.io page and every `parent`,
    `source`, and `target` reference resolves to an existing cell on that page.
28. Confirm the expected page count and export every intended page, not only
    the first page of a multi-page draw.io file.
29. Render a PNG preview for every page intended for static consumption.
30. Inspect each render at normal zoom and crop dense areas for close review.
31. Confirm export completeness: no renderer tile or memory warnings, clipped
    content, blank tile regions, missing short stems, hidden labels, or
    transparent-background surprises.
32. Update the .drawio and static render artifacts together.
33. When the work is a layout-only refactor, compare the semantic signature
    before and after: component and interface identities, component dimensions,
    relationship types, labels, and source/target pairs must remain unchanged.

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

# For the canonical use-case geometry above, every ellipse should match.
rg -A1 'style="ellipse;' diagram.drawio |
  rg 'width="240" height="160"'
~~~

## Precedence

1. The current user's explicit requirements.
2. This README.
3. Existing sample diagrams.
4. General draw.io defaults.

If an older sample conflicts with this document, follow this document.
