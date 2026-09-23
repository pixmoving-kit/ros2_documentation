<span id="add-your-project"></span>

# 添加你的项目

使用下方表单，为你的组织或项目生成 YAML 条目。生成后，可以复制 YAML 片段，并向 `rolling` 分支中的 [adopters.yaml 文件](https://github.com/ros2/ros2_documentation/blob/rolling/source/The-ROS2-Project/Adopters/adopters.yaml)提交 Pull Request。

<span id="policy"></span>

## 收录政策

本列表采用**自行申报、自行确认**的方式。除非收到投诉，否则只对提交的条目进行最低限度的审查。由于通过 Pull Request 提交内容，记录易于审核，必要时也可以在之后清理。

<span id="how-to-contribute"></span>

## 如何提交

1. 填写下方表单。
2. 点击“生成 YAML”，生成片段。
3. 点击“在 GitHub 上创建 PR”，在 GitHub 网页编辑器中打开文件；YAML 会自动复制到剪贴板。
4. 将生成的 YAML 粘贴到文件中 `adopters:` 列表的末尾。
5. 提交更改并创建 Pull Request。

!!! note "说明"

    向 ROS 2 文档仓库提交的所有 Pull Request 都需要[开发者原创声明（DCO）](https://developercertificate.org/)签署。如果使用 GitHub 网页编辑器且缺少签署，[DCO 机器人](https://github.com/apps/dco)会在 PR 下评论，说明如何补充。使用命令行时，可以通过 `git commit --signoff` 签署。

<div class="adopters-form-container">
<form id="adopters-yaml-form">
<div class="form-group">
<label for="field-organization">组织 *</label>
<span class="form-hint">公司或机构名称</span>
<input id="field-organization" placeholder="例如：Acme Robotics Inc." type="text"/>
</div>
<div class="form-group">
<label for="field-organization-url">组织网址</label>
<span class="form-hint">可选</span>
<input id="field-organization-url" placeholder="https://www.example.com" type="url"/>
</div>
<div class="form-group">
<label for="field-project">项目 *</label>
<span class="form-hint">使用 ROS 的具体项目</span>
<input id="field-project" placeholder="例如：自主叉车" type="text"/>
</div>
<div class="form-group">
<label for="field-project-url">项目网址</label>
<span class="form-hint">可选</span>
<input id="field-project-url" placeholder="https://www.example.com/project" type="url"/>
</div>
<div class="form-group">
<label>领域 * <span class="form-hint">（可多选）</span></label>
<div class="domain-checkboxes">
<label><input name="domain" type="checkbox" value="Agriculture"/> 农业</label>
<label><input name="domain" type="checkbox" value="Aerial/Drone"/> 航空／无人机</label>
<label><input name="domain" type="checkbox" value="Automotive"/> 汽车</label>
<label><input name="domain" type="checkbox" value="Components"/> 组件</label>
<label><input name="domain" type="checkbox" value="Construction"/> 建筑</label>
<label><input name="domain" type="checkbox" value="Consumer Robot"/> 消费级机器人</label>
<label><input name="domain" type="checkbox" value="Defense/Government"/> 国防／政府</label>
<label><input name="domain" type="checkbox" value="Education"/> 教育</label>
<label><input name="domain" type="checkbox" value="Energy"/> 能源</label>
<label><input name="domain" type="checkbox" value="Healthcare/Medical"/> 医疗健康</label>
<label><input name="domain" type="checkbox" value="Humanoid"/> 人形机器人</label>
<label><input name="domain" type="checkbox" value="Logistics/Warehouse"/> 物流／仓储</label>
<label><input name="domain" type="checkbox" value="Manufacturing"/> 制造</label>
<label><input name="domain" type="checkbox" value="Marine"/> 海洋</label>
<label><input name="domain" type="checkbox" value="Research"/> 研究</label>
<label><input name="domain" type="checkbox" value="Space"/> 航天</label>
<label><input name="domain" type="checkbox" value="Service Robot"/> 服务机器人</label>
</div>
</div>
<div class="form-group">
<label for="field-date-added">添加日期 *</label>
<span class="form-hint">自动生成（YYYY-MM-DD）</span>
<input id="field-date-added" readonly="" style="width: 120px; background: #e9ecef;" type="text"/>
</div>
<div class="form-group">
<label for="field-country">国家 *</label>
<span class="form-hint">选择一个或多个国家</span>
<div style="display: flex; gap: 0.5rem; align-items: center; flex-wrap: wrap;">
<select id="field-country" style="width: 280px;">
<option value="">-- 选择国家 --</option>
</select>
<button class="btn btn-secondary" id="adopters-add-country-btn" style="margin-top: 0;" type="button">添加</button>
</div>
<div class="adopters-country-tags" id="adopters-selected-countries"></div>
</div>
<div class="form-group">
<label for="field-description">说明 *</label>
<span class="form-hint">简要说明你如何使用 ROS</span>
<textarea id="field-description" placeholder="例如：使用 ROS 2 和 Nav2 实现仓储物流自主导航。"></textarea>
</div>
<div id="adopters-form-errors" style="display: none;"></div>
<button class="btn btn-primary" id="adopters-generate-btn" type="button">生成 YAML</button>
<button class="btn btn-secondary" id="adopters-copy-btn" style="display: none;" type="button">复制到剪贴板</button>
<button class="btn btn-success" id="adopters-open-pr-btn" style="display: none;" type="button">在 GitHub 上创建 PR</button>
<pre id="adopters-yaml-output" style="display: none;"></pre>
</form>
</div>
