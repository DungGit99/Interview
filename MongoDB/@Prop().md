# 🧱 @Prop() là gì
- Trong @nestjs/mongoose, @Prop() là một decorator được sử dụng bên trong class để định nghĩa các field (trường) trong một Schema MongoDB.
- Nó là cú pháp rút gọn và typescript-friendly để xây dựng schema với NestJS.

## 🧩 Cách dùng cơ bản
```ts
import { Prop, Schema, SchemaFactory } from '@nestjs/mongoose';
import { Document } from 'mongoose';

@Schema()
export class User extends Document {
  @Prop({ required: true })
  username: string;

  @Prop()
  email: string;

  @Prop({ default: 'USER' })
  role: string;
}

export const UserSchema = SchemaFactory.createForClass(User);
```
## ⚙️ Các tùy chọn phổ biến của @Prop()
| Tùy chọn 		   | Giải thích |
|----------|----------|
| required: true   | Trường này bắt buộc phải có |
| default: value   | Gán giá trị mặc định  |
| unique: true | 	Không được trùng lặp giá trị |
| type: String   | Xác định kiểu dữ liệu (ít dùng, thường tự infer từ TypeScript) |
| required: true   | Trường này bắt buộc phải có |

### Ví dụ nâng cao:
```ts
@Prop({ required: true, unique: true })
email: string;

@Prop({ enum: ['ADMIN', 'USER'], default: 'USER' })
role: string;
```