# Problem
How pop up Modal in asp.net webforms Gridview.

# Environment
Visual Studio 2010, .Net 4, C#, Web Forms

# How you fixed it
When we add the Modal, then mostly the button click event will not trigger Modal pop up in gridview.

# Solution
Add the javascript to trigger the Modal pop up.

```
<script type='text/javascript'>
                                function openModal() {
                                    $('[id*=myModal]').modal('show');
                                } 
                            </script>
```


```
<asp:GridView ID="gvSearchedDocuments" runat="server" BorderWidth="2px" AutoGenerateColumns="False"
                            DataKeyNames="Doc_ID" Font-Size="9pt" BackColor="White" Font-Names="Arial" CellSpacing="1"
                            Visible="false">
                            <Columns>
                                <asp:TemplateField HeaderText="Select">
                                    <ItemTemplate>
                                        <asp:CheckBox ID="CheckBox1" runat="server" />
                                    </ItemTemplate>
                                    <HeaderStyle BackColor="Lavender" />
                                </asp:TemplateField>
                                <asp:CommandField ShowSelectButton="True" SelectText="View" HeaderText="View">
                                    <HeaderStyle BackColor="Lavender" />
                                </asp:CommandField>
                                <asp:ButtonField CommandName="BtnEdit" Text="Edit" Visible="false">
                                    <HeaderStyle BackColor="Lavender" Font-Names="Arial" />
                                </asp:ButtonField>
                                <asp:TemplateField HeaderText="EditRemarks">
                                    <ItemTemplate>
                                        <asp:LinkButton ID="lnkBtnEdit" runat="server" Text="EditRemarks" OnClick="Display"></asp:LinkButton>
                                    </ItemTemplate>
                                </asp:TemplateField>
                            </Columns>
                        </asp:GridView>





<div class="modal fade" id="myModal" role="dialog">
                            <div class="modal-dialog">
                                <!-- Modal content-->
                                <div class="modal-content">
                                    <div class="modal-header">
                                        <button type="button" class="close" data-dismiss="modal">
                                            &times;</button>
                                        <h4 class="modal-title">
                                            Remarks</h4>
                                    </div>
                                    <div class="modal-body">
                                        <div class="col-lg-12 col-sm-12 col-md-12 col-xs-12">
                                            <div class="form-group">
                                                <asp:Label ID="lblstudent" runat="server" Text="Doc ID: "></asp:Label>
                                                <asp:Label ID="lblstudentid" runat="server" Text=""></asp:Label>
                                            </div>
                                            <div class="form-group">
                                                <asp:TextBox ID="txtRemarks" runat="server" TabIndex="3" MaxLength="75" CssClass="form-control"
                                                    placeholder="Enter/Update Remarks"></asp:TextBox>
                                            </div>
                                            <div class="form-group">
                                                <asp:Label ID="Label4" runat="server" Text="Remarks Status"></asp:Label>
                                                <asp:Label ID="lblRemarksStatus" runat="server" Text=""></asp:Label>
                                            </div>
                                            <div class="form-group">
                                                <asp:Label ID="Label8" runat="server" Text="Reason"></asp:Label>
                                                <asp:TextBox ID="txtReason" runat="server" TabIndex="3" MaxLength="75" CssClass="form-control"
                                                    placeholder="Enter/Update Remarks"></asp:TextBox>
                                            </div>
                                            <div class="form-group">
                                                <asp:Label ID="Label6" runat="server" Text="Added By"></asp:Label>
                                                <asp:Label ID="lblMakeBy" runat="server" Text=""></asp:Label>
                                            </div>
                                            <div class="form-group">
                                                <asp:Label ID="Label5" runat="server" Text="Checked By"></asp:Label>
                                                <asp:Label ID="lblCheckedBy" runat="server" Text=""></asp:Label>
                                            </div>
                                        </div>
                                    </div>
                                    <div class="modal-footer">
                                        <asp:Button ID="btnSave" runat="server" Text="AddUpdate" OnClick="btnSave_Click"
                                            CssClass="btn btn-info" />
                                        <%--<button type="button" class="btn btn-info" data-dismiss="modal">
                                            Close</button>--%>
                                    </div>
                                </div>
                            </div>
                            
                            <script type='text/javascript'>
                                function openModal() {
                                    $('[id*=myModal]').modal('show');
                                } 
                            </script>
                        </div>

```

