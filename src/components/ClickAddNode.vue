<script>
import type { BaseBehaviorOptions, RuntimeContext, IPointerEvent } from '@antv/g6';
import { BaseBehavior, CanvasEvent } from '@antv/g6';

interface ClickAddNodeOptions extends BaseBehaviorOptions {
  fill: string;
}

export class ClickAddNode extends BaseBehavior<ClickAddNodeOptions> {
  static defaultOptions: Partial<ClickAddNodeOptions> = {
    fill: 'red',
  };
  constructor(context: RuntimeContext, options: ClickAddNodeOptions) {
    super(context, Object.assign({}, ClickAddNode.defaultOptions, options));
    this.bindEvents();
  }
  bindEvents() {
    const { graph } = this.context;
    graph.on(CanvasEvent.CLICK, this.addNode);
  }
  private addNode = (event: IPointerEvent) => {
    const { graph } = this.context;
    const { layerX, layerY } = event.nativeEvent as PointerEvent;
    graph.addNodeData([
      {
        id: 'node-' + Date.now(),
        style: { x: layerX, y: layerY, fill: this.options.fill },
      },
    ]);
    graph.draw();
  };
  private unbindEvents() {
    const { graph } = this.context;
    graph.off(CanvasEvent.CLICK, this.addNode);
  }
  public destroy() {
    // 销毁时解绑事件
    this.unbindEvents();
    super.destroy();
  }
}
</script>