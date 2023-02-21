# Problem
How to make button click event as workable inside update panel in asp.net web forms

# Environment
Visual Studio 2019, .Net 4.6.1, C#, Web Forms

# How you fix it
When we define the update panel, then mostly the button click event will not trigger. So we have to add that button click with its ControlID inside the trigger tag

# Solution
Add the button tag inside the Trigger tag

```
<Triggers>
        <asp:PostBackTrigger ControlID="gdvSongs" />
</Triggers>

```


```
<asp:UpdatePanel runat="server" ID="udpFiles">
    <Triggers>
        <asp:PostBackTrigger ControlID="gdvSongs" />
	</Triggers>
    <ContentTemplate>
        <h1 class="hmang-page-header">TRACKS<asp:Button runat="server" Text="Add New Track" OnClientClick="Show();" ID="btnAdd" Visible="false" class="btn btn-primary set-right"></asp:Button></h1>
        <asp:GridView ID="gdvSongs" runat="server" AutoGenerateColumns="false" CssClass="table table-dark table-condensed table-bordered">
            <Columns>
                <asp:TemplateField>
                    <HeaderTemplate>
                        Track Name
                    </HeaderTemplate>
                    <ItemTemplate>
                        <%# Eval("Name") %>
                    </ItemTemplate>
                </asp:TemplateField>
                <asp:TemplateField>
                    <HeaderTemplate>
                        File
                    </HeaderTemplate>
                    <ItemTemplate>
                        <%# Eval("FileName") %>
                    </ItemTemplate>
                </asp:TemplateField>
                <asp:TemplateField>
                    <ItemTemplate>
                        <asp:Button Text="Download" runat="server" CssClass="btn btn-primary" ID="btnDownload" CommandArgument='<%# Eval("ID") %>' OnClick="btnDownload_Click" />
                    </ItemTemplate>
                </asp:TemplateField>
            </Columns>
        </asp:GridView>
    </ContentTemplate>
</asp:UpdatePanel>

```

Similarly, we can add multiple button tags inside the Trigger tag to make them workable.
