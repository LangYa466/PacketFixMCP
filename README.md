# PacketFixMCP
## 喜欢点个Star
## Star History

[![Star History Char](https://api.star-history.com/svg?repos=LangYa466/PacketFixMCP&type=Date)](https://star-history.com/#LangYa466/PacketFixMCP&Date&theme=dark)

### 如果想要给你的客户端加上PacketFix
### 请按照以下步骤修改你的客户端代码：
#### 如果你的客户端没有使用 [ViaMCP](https://github.com/ViaVersionMCP/ViaMCP) 请你先按照它的步骤使用 [ViaMCP](https://github.com/ViaVersionMCP/ViaMCP)

### BlockLadder
#### setBlockBoundsBasedOnState方法
##### 直接替换以下代码
```java
    public void setBlockBoundsBasedOnState(IBlockAccess worldIn, BlockPos pos)
    {
        IBlockState iblockstate = worldIn.getBlockState(pos);

        if (iblockstate.getBlock() == this)
        {
            float f = 0.125F;
            if (ViaLoadingBase.getInstance().getTargetVersion().newerThanOrEqualTo(ProtocolVersion.v1_12_2)) {
                f = 0.1875f;
            }
            switch ((EnumFacing)iblockstate.getValue(FACING))
            {
                case NORTH:
                    this.setBlockBounds(0.0F, 0.0F, 1.0F - f, 1.0F, 1.0F, 1.0F);
                    break;

                case SOUTH:
                    this.setBlockBounds(0.0F, 0.0F, 0.0F, 1.0F, 1.0F, f);
                    break;

                case WEST:
                    this.setBlockBounds(1.0F - f, 0.0F, 0.0F, 1.0F, 1.0F, 1.0F);
                    break;

                case EAST:
                default:
                    this.setBlockBounds(0.0F, 0.0F, 0.0F, f, 1.0F, 1.0F);
            }
        }
    }
```


### BlockFarmland
#### getCollisionBoundingBox
##### 直接替换以下代码
```java
    public AxisAlignedBB getCollisionBoundingBox(World worldIn, BlockPos pos, IBlockState state)
    {
        double mod = 1f;
        if (ViaLoadingBase.getInstance().getTargetVersion().newerThanOrEqualTo(ProtocolVersion.v1_12_2)) {
            mod = 0.9375f;
        }
        return new AxisAlignedBB((double)pos.getX(), (double)pos.getY(), (double)pos.getZ(), (double)(pos.getX() + 1), (double)(pos.getY() + mod), (double)(pos.getZ() + 1));
    }
```

### BlockLilyPad
#### getCollisionBoundingBox
##### 直接替换以下代码
```java
    public AxisAlignedBB getCollisionBoundingBox(World worldIn, BlockPos pos, IBlockState state)
    {
        if (ViaLoadingBase.getInstance().getTargetVersion().newerThanOrEqualTo(ProtocolVersion.v1_12_2)) {
            return new AxisAlignedBB((double)pos.getX() + 0.0625, (double)pos.getY(), (double)pos.getZ() + this.minZ + 0.0625, (double)pos.getX() + 0.9375, (double)pos.getY() + 0.09375, (double)pos.getZ() + 0.9375);
        }
        return new AxisAlignedBB((double)pos.getX() + this.minX, (double)pos.getY() + this.minY, (double)pos.getZ() + this.minZ, (double)pos.getX() + this.maxX, (double)pos.getY() + this.maxY, (double)pos.getZ() + this.maxZ);
    }
```

### EntityRenderer
##### getMouseOver方法
##### 找到这一段代码
```java
        if (this.pointedEntity != null && flag && vec3.distanceTo(vec33) > 3.0D)
                {
                    this.pointedEntity = null;
                    this.mc.objectMouseOver = new MovingObjectPosition(MovingObjectPosition.MovingObjectType.MISS, vec33, (EnumFacing)null, new BlockPos(vec33));
                }
```
##### 替换成这个
```java


                double distance = 3.0D;
                if (ViaLoadingBase.getInstance().getTargetVersion().newerThanOrEqualTo(ProtocolVersion.v1_12_2)) {
                    distance = 2.9D;
                }

                if (this.pointedEntity != null && flag && vec3.distanceTo(vec33) > distance)
                {
                    this.pointedEntity = null;
                    this.mc.objectMouseOver = new MovingObjectPosition(MovingObjectPosition.MovingObjectType.MISS, vec33, (EnumFacing)null, new BlockPos(vec33));
                }
```

### Entity
#### getCollisionBorderSize
##### 直接替换以下代码
```java
    public float getCollisionBorderSize()
    {
        if (ViaLoadingBase.getInstance().getTargetVersion().newerThanOrEqualTo(ProtocolVersion.v1_12_2)) {
            return 0F;
        }
        return 0.1F;
    }
```

### EntityLivingBase
##### onLivingUpdate方法
##### 找到这一段代码
```java
        if (Math.abs(this.motionX) < movementThreshold)
        {
            this.motionX = 0.0D;
        }

        if (Math.abs(this.motionY) < movementThreshold)
        {
            this.motionY = 0.0D;
        }

        if (Math.abs(this.motionZ) < movementThreshold)
        {
            this.motionZ = 0.0D;
        }

```
##### 替换成这个
```java
        double movementThreshold = 0.005D;
        if (ViaLoadingBase.getInstance().getTargetVersion().newerThanOrEqualTo(ProtocolVersion.v1_12_2)) {
            movementThreshold = 0.003;
        }
        
        if (Math.abs(this.motionX) < movementThreshold)
        {
            this.motionX = 0.0D;
        }

        if (Math.abs(this.motionY) < movementThreshold)
        {
            this.motionY = 0.0D;
        }

        if (Math.abs(this.motionZ) < movementThreshold)
        {
            this.motionZ = 0.0D;
        }
```


### C08PacketPlayerBlockPlacement
#### readPacketData
##### 直接替换以下代码
```java
    public void readPacketData(PacketBuffer buf) throws IOException
    {
        this.position = buf.readBlockPos();
        this.placedBlockDirection = buf.readUnsignedByte();
        this.stack = buf.readItemStackFromBuffer();
        if (ViaLoadingBase.getInstance().getTargetVersion().newerThanOrEqualTo(ProtocolVersion.v1_12_2)) {
            this.facingX = (float)buf.readUnsignedByte();
            this.facingY = (float)buf.readUnsignedByte();
            this.facingZ = (float)buf.readUnsignedByte();
        } else {
            this.facingX = (float)buf.readUnsignedByte() / 16.0F;
            this.facingY = (float)buf.readUnsignedByte() / 16.0F;
            this.facingZ = (float)buf.readUnsignedByte() / 16.0F;
        }
    }

```


## 如果你已经完成了以上步骤
# 那么你的客户端已经可以在1.12.2正常游玩不被一些特殊的反作弊检测了
