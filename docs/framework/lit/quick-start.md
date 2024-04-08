---
id: quick-start
title: Quick Start
---


The bare minimum to get started with TanStack Form is to create a `TanstackFormController`:

```ts
interface Employee {
  firstName: string
  lastName: string
}

form = new TanStackFormController<Employee>(this, {
    defaultValues: {
        firstName : '',
        lastName : ''
    },
})

```

In this example `this` references the instance of your `LitElement` in which you want to use TanStack Form.

To wire a form element in your template up with TanStack Form, use the `field` method of `TanStackFormController`:

```ts
import {FieldApi,TanStackFormController,FormOptions} from '@tanstack/lit-form'

@customElement('form-tan-stack')
export class FormTanStackElement extends LitElement {

    form = new TanStackFormController<Employee>(this, {
        defaultValues: {
            firstName : '',
            lastName : ''
        },
    })

    render() {
        return html`
            <form
            id="form"
            @submit=${(e: Event) => {
                e.preventDefault()
            }}
            >
            <h1>TanStack Form - Lit Demo</h1>

            ${this.form.field(
                {
                name: `firstName`,
                validators: {
                    onChange: ({ value }) =>
                    value.length < 3 ? 'Not long enough' : undefined,
                },
                },
                (field: FieldApi<Employee, 'firstName'>) => {
                return html` <div>
                    <label>First Name</label>
                    <input
                    id="firstName"
                    type="text"
                    placeholder="First Name"
                    .value="${field.state.value}"
                    @blur="${() => field.handleBlur()}"
                    @input="${(e: Event) => {
                        const target = e.target as HTMLInputElement
                        field.handleChange(target.value)
                    }}"
                    />
                </div>`
                },
            )}
            ${this.form.field(
                {
                name: `lastName`,
                validators: {
                    onChange: ({ value }) =>
                    value.length < 3 ? 'Not long enough' : undefined,
                },
                },
                (field: FieldApi<Employee, 'lastName'>) => {
                return html` <div>
                    <label>Last Name</label>
                    <input
                    id="lastName"
                    type="text"
                    placeholder="Last Name"
                    .value="${field.state.value}"
                    @blur="${() => field.handleBlur()}"
                    @input="${(e: Event) => {
                        const target = e.target as HTMLInputElement
                        field.handleChange(target.value)
                    }}"
                    />
                </div>`
                },
            )}
            </form>
            <div>
                <button
                type="submit"
                ?disabled=${this.form.api.state.isSubmitting}
                >${this.form.api.state.isSubmitting ? html` Submitting` : 'Submit'}
                </button>
                <button
                type="button"
                id="reset"
                @click=${() => {
                    this.form.api.reset()
                }}
                >
                Reset
                </button>
            </div>
            </form>
            <pre>${JSON.stringify(this.form.api.state, null, 2)}</pre>
        `
    }
}
```

The first parameter of `field` is `FieldOptions` and the second is callback to render your element. Be aware that you need
to handle updating the element and form yourself as seen in the example above.
