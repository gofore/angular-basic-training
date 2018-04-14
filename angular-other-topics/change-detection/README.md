# Zone.js
- Implements concept of zones (inspired by Dart) in JavaScript
- "A Zone is an execution context that persists across async tasks"
- In practice makes it possible to track the asyncronous calls made (HTTP, timers and event listeners) within the zone
- Angular uses zones internally to track changes in state

---

# Change Detection - General
- Idea: project the internal state into UI (DOM in web)
- The internal state consists of JavaScript primitives such as objects, arrays, strings etc.
- Change only possible on asynchronous events such as timeouts or HTTP request

---

# Change Detection - Angular
- Zone.js makes it possible to track all possible change sources
- Components change detector checks the bindings (like `{{name}}` and `(click)`) defined in its template on change
- Bindings are propagated from the root to leaves in the depth first order
- Change detection graph is directed tree and can't contain cycles -> improved performance, predictability and debugging

---

![Change detection](angular-other-topics/change-detection/change-detection-tree.png "Change detection tree")

---
# Change Detection - Simplified Implementation
```typescript
// very simplified version of actual source
class ApplicationRef {
    changeDetectorRefs: ChangeDetectorRef[] = [];

    constructor(private zone: NgZone) {
      this.zone.onTurnDone
        .subscribe(() => this.zone.run(() => this.tick());
    }

    tick() {
      this.changeDetectorRefs
        .forEach((ref) => ref.detectChanges());
    }
}
```
---

# Change Detection - Strategies
- Change detection strategy describes which strategy will be used the next time change detection is triggered
- Angular has six change detection strategies:
  - **CheckOnce**: After calling _detectChanges_ the mode of the change detector will become _Checked_.
  - **Checked**: Change detector should be skipped until its mode changes to _CheckOnce_.
  - **CheckAlways**: After calling _detectChanges_ the mode of the change detector will remain _CheckAlways_.
  - **Detached**: Change detector sub tree is not a part of the main tree and should be skipped.
  - **OnPush**: Change detector's mode will be set to _CheckOnce_ during hydration.
  - **Default**: Change detector's mode will be set to _CheckAlways_ during hydration.
- Setting the strategy:
```typescript
@Component({`changeDetection: ChangeDetectionStrategy.OnPush`})
```

---

# Change Detection - Angular Performance
- Change detection is one of the key functionalities of Angular and thus it is highly optimized
- CDs get VM optimized monomorphic classes generated for them at runtime
- Change detection is always single-pass (stable) because of uni-directional top-to-bottom flow
- More optimizations possible with _immutables_ and _observables_
