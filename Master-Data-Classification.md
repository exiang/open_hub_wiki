
Query by group code:

```php
<?php $trls = $organization->getClassificationsByGroup('trl'); if (!empty($trls)): ?>
<div class="mt-4">
    <h5 class="text-muted text-uppercase"><?php echo Yii::t('app', 'TRL') ?></h5>
    <div>
        <?php foreach ($trls as $trl) : ?>
            <span class="label"><?php echo $trl->renderTitle() ?></span>
        <?php endforeach; ?>
    </div>
</div>
<?php endif; ?>
```